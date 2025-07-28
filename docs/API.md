# API Documentation - Israeli Retirement Calculator

Complete reference for components, functions, and data structures.

## 📋 Table of Contents

- [Core Components](#core-components)
- [Financial Functions](#financial-functions)
- [Utility Functions](#utility-functions)
- [Data Structures](#data-structures)
- [Constants](#constants)
- [Event Handlers](#event-handlers)

## 🧩 Core Components

### `RetirementCalculatorApp`
Main application container managing all state and calculations.

**Props**: None (root component)

**State**:
- `personalInfo: PersonalInfo` - User's personal details
- `currentSavings: CurrentSavings` - Existing account balances  
- `contributions: Contributions` - Monthly contribution amounts
- `assumptions: Assumptions` - Calculation parameters

**Features**:
- localStorage persistence for all state
- Real-time calculation updates
- Automatic pension contribution calculation

```javascript
const RetirementCalculatorApp = () => {
    // State management with localStorage
    const [personalInfo, setPersonalInfo] = useState(() => 
        loadFromLocalStorage('personalInfo', defaultPersonalInfo)
    );
    
    // Memoized calculations
    const calculationResults = useMemo(() => {
        // Complex financial calculations
    }, [personalInfo, currentSavings, contributions, assumptions]);
    
    return (
        <div className="app-container">
            {/* Component tree */}
        </div>
    );
};
```

### `PersonalInfoForm`
Form for collecting user's personal and demographic information.

**Props**:
- `data: PersonalInfo` - Current personal information
- `onChange: (PersonalInfo) => void` - State update callback

**Fields**:
- `currentAge: number` - User's current age (18-80)
- `retirementAge: number` - Planned retirement age (50-80)
- `monthlyIncome: number` - Net monthly salary in ₪
- `gender: 'male' | 'female'` - Gender for retirement age calculation

```javascript
const PersonalInfoForm = ({ data, onChange }) => {
    const handleChange = (field, value) => {
        onChange(prev => ({ ...prev, [field]: Number(value) || value }));
    };

    return (
        <div className="form-section">
            <h3>פרטים אישיים</h3>
            {/* Form inputs with Hebrew labels */}
        </div>
    );
};
```

### `CurrentSavingsForm`
Form for existing pension and savings account balances.

**Props**:
- `data: CurrentSavings` - Current account balances
- `onChange: (CurrentSavings) => void` - State update callback

**Fields**:
- `nationalInsurance: number` - ביטוח לאומי balance
- `pensionFund: number` - קרן פנסיה balance
- `managerInsurance: number` - ביטוח מנהלים balance
- `privateSavings: number` - חיסכון פרטי balance

### `ContributionsForm`
Form for monthly contribution amounts to various accounts.

**Props**:
- `data: Contributions` - Monthly contribution amounts
- `onChange: (Contributions) => void` - State update callback

**Fields**:
- `employeePension: number` - Employee pension contribution (auto-calculated)
- `employerPension: number` - Employer pension contribution (auto-calculated)
- `additionalSavings: number` - Optional additional monthly savings

**Auto-calculation**: Based on `personalInfo.monthlyIncome` and `PENSION_RATES`

### `AssumptionsForm`
Form for investment and economic assumptions.

**Props**:
- `data: Assumptions` - Calculation assumptions
- `onChange: (Assumptions) => void` - State update callback

**Fields**:
- `expectedReturn: number` - Expected annual return (as decimal)
- `inflationRate: number` - Expected annual inflation (as decimal)
- `withdrawalRate: number` - Retirement withdrawal rate (default 0.04)

### `ResultsDisplay`
Display component for calculation results with Hebrew formatting.

**Props**:
- `results: CalculationResults` - Computed retirement projections

**Displays**:
- Total nominal value at retirement
- Inflation-adjusted real value
- Monthly retirement income
- Income replacement ratio

```javascript
const ResultsDisplay = ({ results }) => {
    return (
        <>
            <div className="results-card">
                <h4>סך הכל בפרישה</h4>
                <div className="value hebrew-number">
                    {formatHebrewCurrency(results.totalAtRetirement)}
                </div>
            </div>
            {/* Additional result cards */}
        </>
    );
};
```

### `ChartSection`
Interactive chart component using Chart.js with Hebrew labels.

**Props**:
- `personalInfo: PersonalInfo` - User demographics
- `results: CalculationResults` - Calculation results
- `assumptions: Assumptions` - Economic assumptions

**Features**:
- Savings projection timeline chart
- Hebrew axis labels and legends
- Responsive design for mobile
- Touch gesture support

## 💰 Financial Functions

### `calculateFutureValue(presentValue, monthlyContribution, annualRate, years)`
Calculates future value of savings with regular contributions using compound interest.

**Parameters**:
- `presentValue: number` - Current total savings
- `monthlyContribution: number` - Monthly contribution amount
- `annualRate: number` - Annual interest rate (as decimal)
- `years: number` - Years until retirement

**Returns**: `number` - Future value in nominal terms

**Formula**: 
```
FV = PV × (1 + r)^n + PMT × [((1 + r)^n - 1) / r]
```

**Example**:
```javascript
const futureValue = calculateFutureValue(100000, 1000, 0.05, 25);
// Returns projected savings after 25 years
```

### `adjustForInflation(futureValue, inflationRate, years)`
Adjusts nominal future value for inflation to calculate real purchasing power.

**Parameters**:
- `futureValue: number` - Nominal future value
- `inflationRate: number` - Annual inflation rate (as decimal)
- `years: number` - Years of inflation adjustment

**Returns**: `number` - Real value in today's purchasing power

**Formula**:
```
Real_Value = Nominal_Value / (1 + inflation_rate)^years
```

### `calculateMonthlyRetirementIncome(totalSavings, withdrawalRate)`
Calculates sustainable monthly income from retirement savings.

**Parameters**:
- `totalSavings: number` - Total retirement savings
- `withdrawalRate: number` - Annual withdrawal rate (default 0.04)

**Returns**: `number` - Monthly retirement income

**Default**: 4% withdrawal rule (Trinity Study)

### `calculateReplacementRatio(retirementIncome, preRetirementIncome)`
Calculates what percentage of pre-retirement income is replaced.

**Parameters**:
- `retirementIncome: number` - Monthly retirement income
- `preRetirementIncome: number` - Pre-retirement monthly income

**Returns**: `number` - Replacement ratio as percentage

## 🛠️ Utility Functions

### `formatHebrewCurrency(amount)`
Formats numbers as Israeli currency with Hebrew locale.

**Parameters**:
- `amount: number` - Amount to format

**Returns**: `string` - Formatted currency string (e.g., "₪123,456")

**Uses**: `Intl.NumberFormat('he-IL', { style: 'currency', currency: 'ILS' })`

### `formatHebrewNumber(number)`
Formats numbers with Hebrew thousands separators.

**Parameters**:
- `number: number` - Number to format

**Returns**: `string` - Formatted number string

### `saveToLocalStorage(key, data)`
Safely saves data to browser localStorage with error handling.

**Parameters**:
- `key: string` - Storage key
- `data: any` - Data to store (will be JSON stringified)

**Error Handling**: Logs errors to console, continues execution

### `loadFromLocalStorage(key, defaultValue)`
Safely loads data from localStorage with fallback to default.

**Parameters**:
- `key: string` - Storage key
- `defaultValue: any` - Fallback value if key not found

**Returns**: Parsed data or defaultValue

## 📊 Data Structures

### `PersonalInfo`
```typescript
interface PersonalInfo {
    currentAge: number;      // 18-80
    retirementAge: number;   // 50-80  
    monthlyIncome: number;   // Net monthly salary in ₪
    gender: 'male' | 'female';
}
```

### `CurrentSavings`
```typescript
interface CurrentSavings {
    nationalInsurance: number;  // ביטוח לאומי balance
    pensionFund: number;        // קרן פנסיה balance
    managerInsurance: number;   // ביטוח מנהלים balance
    privateSavings: number;     // חיסכון פרטי balance
}
```

### `Contributions`
```typescript
interface Contributions {
    employeePension: number;    // Auto-calculated from salary
    employerPension: number;    // Auto-calculated from salary  
    additionalSavings: number;  // User-defined additional savings
}
```

### `Assumptions`
```typescript
interface Assumptions {
    expectedReturn: number;     // Annual return rate (0.0-1.0)
    inflationRate: number;      // Annual inflation rate (0.0-1.0)
    withdrawalRate: number;     // Retirement withdrawal rate (default 0.04)
}
```

### `CalculationResults`
```typescript
interface CalculationResults {
    totalAtRetirement: number;  // Nominal value at retirement
    realValue: number;          // Inflation-adjusted value
    monthlyIncome: number;      // Monthly retirement income
    replacementRatio: number;   // Income replacement percentage
}
```

## 🔢 Constants

### `PENSION_RATES`
Israeli pension system contribution rates.

```javascript
const PENSION_RATES = {
    employee_pension: 0.06,              // 6% employee contribution
    employer_pension: 0.065,             // 6.5% employer contribution
    manager_insurance_employee: 0.025,   // 2.5% manager employee
    manager_insurance_employer: 0.075,   // 7.5% manager employer
    national_insurance_rate: 0.12        // 12% social security (simplified)
};
```

### Default State Values
```javascript
const DEFAULT_PERSONAL_INFO = {
    currentAge: 30,
    retirementAge: 67,
    monthlyIncome: 15000,
    gender: 'male'
};

const DEFAULT_CURRENT_SAVINGS = {
    nationalInsurance: 50000,
    pensionFund: 100000,
    managerInsurance: 0,
    privateSavings: 20000
};

const DEFAULT_ASSUMPTIONS = {
    expectedReturn: 0.05,    // 5% annual return
    inflationRate: 0.025,    // 2.5% annual inflation
    withdrawalRate: 0.04     // 4% withdrawal rate
};
```

## 🎛️ Event Handlers

### Form Change Handlers
All form components use a similar pattern for handling input changes:

```javascript
const handleChange = (field, value) => {
    onChange(prev => ({ 
        ...prev, 
        [field]: Number(value) || value 
    }));
};
```

**Features**:
- Automatic number conversion for numeric fields
- Preserves string values for non-numeric fields
- Immutable state updates using spread operator

### Auto-calculation Effects
Automatic pension contribution calculation:

```javascript
useEffect(() => {
    const monthlyIncome = personalInfo.monthlyIncome;
    setContributions(prev => ({
        ...prev,
        employeePension: monthlyIncome * PENSION_RATES.employee_pension,
        employerPension: monthlyIncome * PENSION_RATES.employer_pension
    }));
}, [personalInfo.monthlyIncome]);
```

### localStorage Persistence
Automatic saving to localStorage on state changes:

```javascript
useEffect(() => {
    saveToLocalStorage('personalInfo', personalInfo);
}, [personalInfo]);
```

## 🎨 CSS Classes

### Layout Classes
- `.app-container` - Main application wrapper
- `.main-content` - Grid layout container
- `.form-section` - Individual form containers
- `.results-section` - Results display area

### Form Classes
- `.form-group` - Individual form field wrapper
- `.form-group label` - Form field labels
- `.form-group input` - Input field styling
- `.form-group select` - Dropdown styling

### Results Classes
- `.results-card` - Individual result display cards
- `.results-card .value` - Large result values
- `.results-card .subtitle` - Additional result information

### Utility Classes
- `.hebrew-number` - Hebrew number formatting
- `.loading` - Loading spinner animation
- `.sr-only` - Screen reader only content

## 🌐 Internationalization

### Hebrew RTL Support
- **Direction**: All components inherit `direction: rtl`
- **Typography**: Hebrew fonts with proper rendering
- **Layout**: Right-to-left form flow and navigation
- **Numbers**: Israeli number and currency formatting

### Accessibility Features
- ARIA labels in Hebrew
- Screen reader compatibility
- High contrast mode support
- Keyboard navigation (RTL-aware)

## 🔧 Extension Points

### Adding New Account Types
1. Update `CurrentSavings` interface
2. Add form fields in `CurrentSavingsForm`
3. Include in total calculation logic
4. Update Hebrew labels

### Custom Calculation Methods
1. Create new calculation functions
2. Add to `calculationResults` useMemo
3. Update `ResultsDisplay` component
4. Test with various scenarios

### Additional Languages
1. Create language toggle component
2. Duplicate all Hebrew text strings
3. Update number formatting functions
4. Modify CSS for text direction changes

This API documentation provides complete technical reference for maintaining and extending the Israeli Retirement Calculator.