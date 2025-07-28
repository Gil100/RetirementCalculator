# מחשבון פנסיה ישראלי - Israeli Retirement Calculator

A comprehensive retirement planning calculator designed specifically for the Israeli pension system, featuring a native Hebrew RTL interface and accurate financial modeling.

## 🌟 Features

### 💰 Israeli Pension System Integration
- **ביטוח לאומי (National Insurance)**: Social security calculations
- **קרן פנסיה (Pension Fund)**: Mandatory employer/employee contributions (6% + 6.5%)
- **ביטוח מנהלים (Manager Insurance)**: Executive insurance plans (up to 2.5% + 7.5%)
- **חיסכון פרטי (Private Savings)**: Additional voluntary savings

### 🌐 Native Hebrew Interface
- **RTL Layout**: Complete right-to-left user interface
- **Hebrew Typography**: Alef and Open Sans Hebrew fonts
- **Financial Terminology**: Authentic Hebrew financial terms
- **Number Formatting**: Israeli currency (₪) and number formats
- **Date Formatting**: Hebrew calendar integration

### 📱 Mobile-First Responsive Design
- **Touch-Friendly**: Optimized for mobile devices
- **Breakpoint Strategy**: Mobile (320px+), Tablet (768px+), Desktop (1024px+)
- **Hebrew RTL Mobile**: Right-to-left swipe navigation
- **Accessibility**: Screen reader support and high contrast mode

### 📊 Advanced Financial Modeling
- **Future Value Calculations**: Compound interest with regular contributions
- **Inflation Adjustments**: Real purchasing power projections
- **Retirement Income**: 4% withdrawal rule with replacement ratio analysis
- **Interactive Charts**: Savings projection visualizations with Chart.js

### 💾 Data Persistence
- **localStorage**: Automatic saving of user inputs
- **Session Recovery**: Resume calculations across browser sessions
- **Export Ready**: Print-friendly layouts

## 🚀 Quick Start

### Option 1: Direct Usage
1. Download `israeli-retirement-calculator.html`
2. Open in any modern web browser
3. Start planning your retirement in Hebrew!

### Option 2: GitHub Pages Deployment
1. Fork this repository
2. Go to Settings → Pages
3. Select "Deploy from a branch" → main
4. Your calculator will be available at `https://yourusername.github.io/RetirementCalculator/israeli-retirement-calculator.html`

## 🏗️ Technical Architecture

### Single HTML File Design
```
israeli-retirement-calculator.html
├── Hebrew RTL CSS Framework (embedded)
├── React Components (CDN)
├── Financial Calculation Engine
├── Chart.js Visualizations
└── localStorage Persistence
```

### Technology Stack
- **Frontend**: React 18 via CDN
- **Styling**: Custom Hebrew RTL CSS
- **Charts**: Chart.js with Hebrew labels
- **Fonts**: Google Fonts (Alef, Open Sans Hebrew)
- **Storage**: Browser localStorage
- **Deployment**: Static HTML (GitHub Pages ready)

### Key Components
- `RetirementCalculatorApp` - Main application container
- `PersonalInfoForm` - Age, income, and personal details
- `CurrentSavingsForm` - Existing pension and savings accounts
- `ContributionsForm` - Monthly contribution calculations
- `AssumptionsForm` - Investment returns and inflation
- `ResultsDisplay` - Retirement projections and income
- `ChartSection` - Interactive data visualizations

## 💡 Usage Guide

### 1. Personal Information (פרטים אישיים)
- **Current Age (גיל נוכחי)**: Your current age
- **Retirement Age (גיל פרישה מתוכנן)**: Planned retirement age
- **Monthly Income (הכנסה חודשית נטו)**: Net monthly salary in ₪
- **Gender (מין)**: Affects retirement age calculations

### 2. Current Savings (חיסכון נוכחי)
- **National Insurance (ביטוח לאומי)**: Current social security balance
- **Pension Fund (קרן פנסיה)**: Existing pension fund balance
- **Manager Insurance (ביטוח מנהלים)**: Executive insurance balance
- **Private Savings (חיסכון פרטי)**: Additional savings and investments

### 3. Monthly Contributions (הפרשות חודשיות)
- **Employee Pension (הפרשת עובד)**: Automatically calculated (6% of salary)
- **Employer Pension (הפרשת מעביד)**: Automatically calculated (6.5% of salary)
- **Additional Savings (חיסכון נוסף)**: Optional extra monthly savings

### 4. Assumptions (הנחות חישוב)
- **Expected Return (תשואה שנתית צפויה)**: Annual investment return percentage
- **Inflation Rate (שיעור אינפלציה)**: Expected annual inflation
- **Withdrawal Rate (שיעור משיכה בפרישה)**: Retirement withdrawal percentage (default 4%)

### 5. Results (תוצאות)
- **Total at Retirement (סך הכל בפרישה)**: Nominal value at retirement
- **Real Value (ערך ריאלי)**: Inflation-adjusted purchasing power
- **Monthly Income (הכנסה חודשית בפרישה)**: Projected monthly retirement income
- **Replacement Ratio (יחס החלפה)**: Percentage of pre-retirement income

## 📊 Financial Calculations

### Future Value Formula
```javascript
FV = PV × (1 + r)^n + PMT × [((1 + r)^n - 1) / r]
```
Where:
- `PV` = Present Value (current savings)
- `PMT` = Monthly contributions
- `r` = Monthly interest rate
- `n` = Number of months until retirement

### Inflation Adjustment
```javascript
Real_Value = Nominal_Value / (1 + inflation_rate)^years
```

### Israeli Pension Rates
```javascript
const PENSION_RATES = {
    employee_pension: 0.06,        // 6% employee contribution
    employer_pension: 0.065,       // 6.5% employer contribution  
    manager_insurance_employee: 0.025,  // 2.5% executive employee
    manager_insurance_employer: 0.075,  // 7.5% executive employer
    national_insurance_rate: 0.12       // 12% social security
};
```

## 🎨 Hebrew Localization Features

### RTL Typography
- **Direction**: `html { direction: rtl; }`
- **Font Stack**: `'Open Sans Hebrew', 'Alef', Arial, sans-serif`
- **Text Alignment**: Right-aligned by default
- **Number Format**: Hebrew thousands separators

### Financial Terminology
| English | Hebrew | Context |
|---------|--------|---------|
| Retirement Calculator | מחשבון פנסיה | Main title |
| Current Age | גיל נוכחי | Form input |
| Retirement Age | גיל פרישה | Form input |
| Monthly Income | הכנסה חודשית | Salary field |
| Pension Fund | קרן פנסיה | Account type |
| National Insurance | ביטוח לאומי | Social security |
| Manager Insurance | ביטוח מנהלים | Executive plans |
| Private Savings | חיסכון פרטי | Personal savings |

### Responsive RTL Design
- **Mobile**: Single-column layout with touch-friendly inputs
- **Tablet**: Two-column forms with proper RTL flow
- **Desktop**: Full dashboard with RTL navigation
- **Charts**: Hebrew labels and RTL legends

## 🔧 Customization

### Modifying Pension Rates
Update the `PENSION_RATES` constant in the HTML file:
```javascript
const PENSION_RATES = {
    employee_pension: 0.06,    // Adjust employee contribution rate
    employer_pension: 0.065,   // Adjust employer contribution rate
    // ... other rates
};
```

### Adding New Languages
1. Duplicate form labels in the HTML
2. Add language toggle functionality
3. Update number formatting functions
4. Modify CSS for different text directions

### Customizing Visual Theme
Modify CSS variables in the `<style>` section:
```css
:root {
    --primary-color: #4CAF50;
    --secondary-color: #667eea;
    --background-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

## 🌐 Browser Compatibility

### Supported Browsers
- ✅ **Chrome 90+**: Full RTL and Chart.js support
- ✅ **Firefox 88+**: Complete Hebrew font rendering
- ✅ **Safari 14+**: iOS and macOS RTL support
- ✅ **Edge 90+**: Windows Hebrew localization
- ⚠️ **IE**: Not supported (requires modern JavaScript)

### Required Features
- ES6+ JavaScript support
- CSS Grid and Flexbox
- localStorage API
- Hebrew font rendering
- RTL text direction support

## 📚 Development

### File Structure
```
RetirementCalculator/
├── israeli-retirement-calculator.html  # Main application
├── README.md                          # Documentation
└── docs/                             # Additional documentation
    ├── API.md                        # Component API reference
    ├── DEPLOYMENT.md                 # Deployment guide
    └── LOCALIZATION.md              # Hebrew features guide
```

### Making Changes
1. Edit the HTML file directly
2. Test in multiple browsers
3. Validate Hebrew text rendering
4. Check mobile responsiveness
5. Test RTL layout integrity

### Testing Checklist
- [ ] Hebrew text displays correctly
- [ ] RTL layout works on all screen sizes
- [ ] Financial calculations are accurate
- [ ] Charts render with Hebrew labels
- [ ] localStorage persistence works
- [ ] Mobile touch interactions function
- [ ] Accessibility features work with screen readers

## 🤝 Contributing

### Ways to Contribute
1. **Bug Reports**: Israeli pension system updates
2. **Feature Requests**: Additional account types or calculations
3. **Localization**: Other languages or regional variations
4. **Accessibility**: Improved screen reader support
5. **Performance**: Optimization suggestions

### Contribution Guidelines
1. Maintain Hebrew RTL compatibility
2. Follow existing code formatting
3. Test across multiple devices
4. Validate financial accuracy
5. Update documentation

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🙏 Acknowledgments

- **Israeli Ministry of Finance**: Pension system regulations
- **Bank of Israel**: Inflation and economic data
- **Google Fonts**: Hebrew typography support
- **Chart.js Community**: Data visualization tools
- **React Team**: Component framework

## 📞 Support

For questions about Israeli pension calculations or technical support:

- **Issues**: [GitHub Issues](https://github.com/yourusername/RetirementCalculator/issues)
- **Documentation**: See `/docs` folder for detailed guides
- **Hebrew Support**: Native Hebrew RTL interface included

---

**בהצלחה בתכנון הפרישה שלכם! Good luck with your retirement planning!** 🎯