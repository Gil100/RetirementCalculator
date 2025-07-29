# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

This is a single-file HTML application with no build system. No npm/yarn commands needed.

### Local Development
```bash
# Serve locally for development
python -m http.server 8000
# OR
npx http-server -p 8000

# Access at: http://localhost:8000/israeli-retirement-calculator.html
```

### Testing
- **Manual Testing**: Open in multiple browsers (Chrome, Firefox, Safari, Edge)
- **Hebrew RTL Testing**: Verify right-to-left layout on all screen sizes
- **Mobile Testing**: Test touch interactions and responsive design
- **Accessibility Testing**: Use screen readers with Hebrew content

### Deployment
- **GitHub Pages**: Enable in repository settings, access at `https://username.github.io/RetirementCalculator/israeli-retirement-calculator.html`
- **Static Hosting**: Upload `israeli-retirement-calculator.html` to any web server
- **CDN Platforms**: Deploy to Netlify, Vercel, or Firebase Hosting

## Code Architecture

### Single HTML File Design
The entire application is contained in `israeli-retirement-calculator.html` with embedded:
- **CSS**: Custom Hebrew RTL framework with responsive design
- **JavaScript**: React components loaded via CDN
- **Dependencies**: React 18, Chart.js, Google Fonts for Hebrew typography

### Core React Components
- `RetirementCalculatorApp` - Main application with state management and localStorage persistence
- `PersonalInfoForm` - Age, income, gender inputs with Hebrew labels
- `CurrentSavingsForm` - Existing pension fund, social security, private savings
- `ContributionsForm` - Monthly contributions (auto-calculated from salary)
- `AssumptionsForm` - Investment returns, inflation, withdrawal rates
- `ResultsDisplay` - Retirement projections with Hebrew currency formatting
- `ChartSection` - Interactive Chart.js visualizations with Hebrew labels

### State Management
- **localStorage**: Automatic persistence across browser sessions
- **Version Control**: `STORAGE_VERSION = '1.3'` handles data migration
- **Real-time Updates**: All calculations update automatically on input changes

### Financial Calculation Engine
```javascript
// Core Israeli pension rates
const PENSION_RATES = {
    employee_pension: 0.06,        // 6% employee contribution
    employer_pension: 0.065,       // 6.5% employer contribution  
    manager_insurance_employee: 0.025,  // 2.5% executive employee
    manager_insurance_employer: 0.075,  // 7.5% executive employer
    national_insurance_rate: 0.12       // 12% social security
};

// Future value formula: FV = PV × (1 + r)^n + PMT × [((1 + r)^n - 1) / r]
const calculateFutureValue = (presentValue, monthlyContribution, annualRate, years)
```

### Hebrew RTL System
- **Direction**: `html { direction: rtl; }` with inheritance
- **Typography**: `'Open Sans Hebrew', 'Alef', Arial, sans-serif`
- **Currency**: Israeli Shekel (₪) with Hebrew number formatting
- **Layout**: Right-to-left form flow, navigation, and chart legends

## Key Features

### Israeli Pension System Integration
- **ביטוח לאומי (National Insurance)**: Social security calculations
- **קרן פנסיה (Pension Fund)**: Mandatory employer/employee contributions
- **ביטוח מנהלים (Manager Insurance)**: Executive insurance plans
- **חיסכון פרטי (Private Savings)**: Additional voluntary savings

### Responsive Hebrew Interface
- **Mobile-First**: Touch-friendly inputs with proper RTL mobile flow
- **Breakpoints**: Mobile (320px+), Tablet (768px+), Desktop (1024px+)
- **Accessibility**: Screen reader support, high contrast mode, WCAG compliance

### Financial Modeling
- **Future Value**: Compound interest with regular contributions
- **Inflation Adjustment**: Real purchasing power calculations
- **Withdrawal Rate**: 4% rule with replacement ratio analysis
- **Interactive Charts**: Savings projections with Hebrew labels

## Development Guidelines

### Working with Hebrew RTL
- All text must be right-to-left compatible
- Use Hebrew financial terminology consistently
- Test number formatting with Israeli currency (₪)
- Ensure form layouts work with RTL text direction

### Component Patterns
```javascript
// Standard form input pattern
const handleChange = (field, value) => {
    onChange(prev => ({ ...prev, [field]: Number(value) || value }));
};

// Hebrew currency formatting
const formatHebrewCurrency = (amount) => {
    return new Intl.NumberFormat('he-IL', { 
        style: 'currency', 
        currency: 'ILS' 
    }).format(amount);
};
```

### State Persistence
All form data automatically saves to localStorage with version control. Check `STORAGE_VERSION` when making breaking changes to data structure.

### Chart Configuration
Chart.js configured for Hebrew labels and RTL layouts. Use Hebrew text for all chart elements and ensure proper RTL legend positioning.

## Browser Support

- ✅ **Chrome 90+**: Full RTL and Chart.js support
- ✅ **Firefox 88+**: Complete Hebrew font rendering
- ✅ **Safari 14+**: iOS and macOS RTL support
- ✅ **Edge 90+**: Windows Hebrew localization
- ❌ **IE**: Not supported (requires modern JavaScript)

## File Structure

```
RetirementCalculator/
├── israeli-retirement-calculator.html  # Main application (all code)
├── index.html                          # Duplicate for GitHub Pages
├── README.md                          # Project documentation
└── docs/                             # Additional documentation
    ├── API.md                        # Component reference
    ├── DEPLOYMENT.md                 # Deployment guide
    └── LOCALIZATION.md              # Hebrew features guide
```

## Customization Points

### Modifying Pension Rates
Update `PENSION_RATES` constant to reflect current Israeli pension regulations.

### Adding Account Types
1. Update data structures in relevant forms
2. Add Hebrew labels and tooltips
3. Include in calculation logic
4. Test with various scenarios

### Internationalization
The codebase is structured for Hebrew but can be extended for other languages by:
1. Adding language toggle functionality
2. Duplicating text strings
3. Updating number formatting functions
4. Modifying CSS for text direction changes

## Security & Privacy

- **Client-Side Only**: No server communication, all data stays in browser
- **No External Tracking**: No analytics or third-party data collection
- **localStorage Only**: User data never leaves their device
- **HTTPS Required**: All deployments should use HTTPS for security