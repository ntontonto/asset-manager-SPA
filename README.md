# Household Analytics Dashboard

A sophisticated single-page application (SPA) for comprehensive household financial analytics with a 3-layer structure: **Household**, **Partner 1**, and **Partner 2**. This dashboard provides advanced data visualization, AI-powered insights, and multi-format data import capabilities.

> **White-label Solution**: Fully customizable for any couple or household tracking shared and individual finances.

![Dashboard Preview](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)
![License](https://img.shields.io/badge/License-MIT-blue)

## 🌟 Features

### 📊 **23 Interactive Charts**
- **Household Analytics**: Asset growth trends, income vs expenses, savings rate
- **Expense Breakdown**: Category-wise spending analysis (rent, food, utilities, etc.)
- **Partner Comparison**: Side-by-side financial performance analysis
- **Individual Dashboards**: Dedicated money flow analysis for each partner
- **Portfolio Analysis**: Cash vs investment ratio tracking
- **Financial Health Score**: Radar chart with 5 key metrics

### 🤖 **AI-Powered Insights**
- Google Gemini AI integration for financial advisory
- Automated analysis of savings patterns
- Personalized recommendations for expense optimization
- Investment balance suggestions

### 📁 **Flexible Data Import**
- **Excel Support**: `.xlsx`, `.xls` with multi-sheet recognition
- **CSV Support**: Auto-detection of Shift_JIS/UTF-8 encoding
- **Smart Column Mapping**: Flexible field name recognition (Japanese/English)
- **Multi-Sheet Processing**: Separate sheets for Partner 1, Partner 2, and Aggregate data

### 📤 **Export Capabilities**
- **PDF Reports**: Complete dashboard export with all charts
- **JSON Export**: Raw filtered data for further analysis

### 🎨 **Modern UI/UX**
- Responsive design (mobile/tablet/desktop)
- Sticky navigation with smooth scrolling
- Interactive date range filtering
- 1-column / 2-column layout toggle
- Professional gradient design with Tailwind CSS

## 🚀 Getting Started

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Internet connection (for CDN libraries)
- Google Gemini API key (optional, for AI features)

### Installation

1. **Clone or Download**
   ```bash
   git clone https://github.com/yourusername/asset-manager-SPA.git
   cd asset-manager-SPA
   ```

2. **Open in Browser**
   ```bash
   # Open directly in browser
   open index.html
   # or
   chrome index.html
   ```

3. **Optional: Setup Local Server**
   ```bash
   # Using Python
   python -m http.server 8000
   
   # Using Node.js
   npx http-server
   ```

### Configuration

#### Setting up AI Integration (Optional)

1. Get a free API key from [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Open `index.html` in a text editor
3. Find line 420 and update the partner configuration:
   ```javascript
   const PARTNER_CONFIG = {
       partner1: {
           name: 'k',              // Column name in Excel/CSV
           displayName: 'Partner 1', // Display name in charts
           color: '#2563eb'
       },
       partner2: {
           name: 'm',              // Column name in Excel/CSV  
           displayName: 'Partner 2', // Display name in charts
           color: '#e11d48'
       }
   };
   ```

4. Optionally, add your Google Gemini API key for AI features (line 423):
   ```javascript
   const apiKey = "YOUR_GEMINI_API_KEY_HERE";
   ```

## 📖 Usage

### 1. Prepare Your Data

> **Important**: Partner column names must match the `PARTNER_CONFIG` settings in `index.html`. Default names are `'k'` and `'m'`.

Your data file should include the following columns:

#### For Single-Sheet CSV/Excel:
```
Month, 収入総額, 収入k, 収入m, 共通費の負担額_k, 共通費の負担額_m, 
個人の支出 (Partner 1), 個人の支出 (Partner 2), 家賃, 食費, 光熱費, 通信費, 
二人での資産総額, 資産総額_k, 資産総額_m, 相殺用_m->k
```

#### For Multi-Sheet Excel:
- **Sheet 1 (Aggregate)**: Household totals and combined data
- **Sheet 2 (k or partner1)**: Partner 1's individual transactions
- **Sheet 3 (m or partner2)**: Partner 2's individual transactions

#### Example Data Structure:
| Month   | 収入総額 | 収入k | 収入m | 家賃  | 食費  | 二人での資産総額 |
|---------|---------|------|------|------|------|--------------|
| 2024-01 | 500000  | 300000   | 200000  | 80000| 40000| 5000000      |
| 2024-02 | 520000  | 310000   | 210000  | 80000| 42000| 5100000      |

### 2. Load Data

1. Click **"データを読み込む (Excel / CSV)"** on the splash screen
2. Select your data file
3. Wait for processing (loading overlay will appear)
4. Dashboard will automatically appear with all charts

### 3. Analyze Data

- **Navigate**: Use quick navigation tiles or sticky nav to jump to sections
- **Filter**: Use date range picker in header to filter data
- **Layout**: Toggle between 1-column (detailed) or 2-column (compact) view
- **AI Insights**: Scroll to bottom for AI-generated financial recommendations

### 4. Export Results

- **PDF**: Click "PDF保存" to download full report
- **JSON**: Click "JSON保存" to export raw data

## 🏗️ Architecture

### Tech Stack
- **Frontend**: Vanilla JavaScript (ES6+)
- **Styling**: Tailwind CSS (CDN)
- **Charts**: Chart.js v4 with datalabels plugin
- **Data Parsing**: PapaCSV (CSV) + SheetJS (Excel)
- **Export**: jsPDF + html2canvas
- **AI**: Google Gemini API (gemini-2.5-flash)
- **Icons**: Lucide Icons
- **Markdown**: Marked.js

### File Structure
```
asset-manager-SPA/
├── index.html          # Single-file application (1327 lines)
├── README.md           # This file
└── .git/               # Git repository
```

### Data Model

The application processes data into a 3-layer structure:

```javascript
{
  date: Date,
  k: {  // Partner 1's data
    income: number,
    burden: number,    // household contribution
    spending: number,  // personal expenses
    balance: number,   // savings
    cash: number,
    invest: number
  },
  m: {  // Partner 2's data
    // ... same structure as Partner 1
  },
  a: {  // Aggregate (household)
    income: number,
    expense: number,
    balance: number,
    assets: number,
    rent: number,
    food: number,
    utility: number,
    internet: number,
    furniture: number,
    entPair: number,
    other: number,
    assetsK: number,  // Partner 1's total assets
    assetsM: number,  // Partner 2's total assets
    adjustment: number  // settlement between partners
  }
}
```

## 📊 Chart Types

| ID   | Chart Name | Type | Description |
|------|-----------|------|-------------|
| c1   | 世帯総資産の成長トレンド | Area | Total household asset growth |
| c2   | 家計簿収支 | Combo | Income vs expenses with balance line |
| c3   | 費目別支出の積み上げ推移 | Stacked Area | Category-wise expense breakdown |
| c4   | 家計黒字率の推移 | Line | Savings rate over time |
| c7   | 個人の余剰資金比較 | Bar | Partner surplus comparison |
| c9   | Partner 1 マネーフロー | Combo | Partner 1's income/expense/savings flow |
| c11  | パートナー別資産総額推移 | Line | Asset comparison by partner |
| c12  | Partner 1個人支出推移 | Bar | Partner 1's personal spending |
| c13  | Partner 2 マネーフロー | Combo | Partner 2's income/expense/savings flow |
| c16  | Partner 2個人支出推移 | Bar | Partner 2's personal spending |
| c17  | パートナー間精算額 | Bar | Settlement between partners |
| c18  | エンゲル係数 | Line | Food expense ratio |
| c19  | 通信費推移 | Line | Communication costs |
| c20  | 家計健全度スコア | Radar | Financial health metrics |
| c21  | Partner 1資産ポートフォリオ | Stacked Bar | Partner 1's cash vs investment |
| c22  | Partner 2資産ポートフォリオ | Stacked Bar | Partner 2's cash vs investment |
| c23  | 現金保有率の推移 | Line | Cash ratio trends |

## 🎯 Use Cases

- **Couples Managing Shared Finances**: Track individual and joint expenses
- **Household Budget Planning**: Analyze spending patterns and savings
- **Investment Tracking**: Monitor portfolio allocation (cash vs investment)
- **Financial Goal Setting**: Use AI insights for optimization strategies
- **Settlement Tracking**: Keep records of partner-to-partner transactions

## 🔧 Customization

### Modify Partner Names and Colors
Edit the `PARTNER_CONFIG` object (line ~410):
```javascript
const PARTNER_CONFIG = {
    partner1: {
        name: 'john',           // Column name in Excel/CSV
        displayName: 'John',    // Display name in charts
        color: '#2563eb'        // Blue
    },
    partner2: {
        name: 'jane',           // Column name in Excel/CSV
        displayName: 'Jane',    // Display name in charts
        color: '#e11d48'        // Rose
    }
};
```

### Modify Chart Color Scheme
Edit the `COLORS` object (line ~440):
```javascript
const COLORS = {
    partner1: PARTNER_CONFIG.partner1.color, // Uses config
    partner2: PARTNER_CONFIG.partner2.color, // Uses config
    total: '#10b981',   // Emerald
    expense: '#f59e0b', // Amber
    // ... customize other colors
};
```

### Add New Charts
Add to `globalChartDefinitions` array in `renderCharts()` function:
```javascript
{
    id: 'c24',
    title: 'Your Chart Title',
    type: 'line',  // or 'bar', 'radar', etc.
    data: {
        labels: labels,
        datasets: [/* your datasets */]
    }
}
```

### Adjust Column Mapping
Modify the `getVal()` calls in `processData()` function to match your column names.

## 🐛 Troubleshooting

### Issue: Blank/Empty Page
- **Solution**: Make sure the page is opened via a web server (not just `file://`)
- Check browser console for errors (F12)
- Ensure all CDN libraries are loading (check network tab)

### Issue: Data Not Loading
- **Solution**: Verify your data file has required columns
- Check date format (should be parseable by JavaScript Date)
- Ensure numeric values don't have special characters except `-`, `.`, `,`

### Issue: AI Insights Show Error
- **Solution**: Add valid Google Gemini API key
- Check API quota limits
- Verify internet connection

### Issue: Charts Not Rendering
- **Solution**: Ensure Chart.js and dependencies are loaded
- Check browser console for specific errors
- Try refreshing the page

## 🚀 Performance

- **Load Time**: ~2-3 seconds (depending on CDN speed)
- **Data Processing**: Handles 100+ months of data smoothly
- **Chart Rendering**: Uses canvas for optimal performance
- **PDF Export**: May take 10-30 seconds for 23 charts

## 📱 Browser Support

| Browser | Version | Status |
|---------|---------|--------|
| Chrome  | 90+     | ✅ Full Support |
| Firefox | 88+     | ✅ Full Support |
| Safari  | 14+     | ✅ Full Support |
| Edge    | 90+     | ✅ Full Support |

## 🔒 Security & Privacy

- **Local Processing**: All data processing happens in your browser
- **No Data Upload**: Your financial data never leaves your device
- **API Key**: AI feature requires API key (stored client-side only)
- **No Backend**: Pure client-side application

## 📄 License

This project is licensed under the MIT License - feel free to use it for personal or commercial purposes.

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest new features
- Submit pull requests
- Improve documentation

## 📞 Support

For questions or issues:
- Open an issue on GitHub
- Check browser console for error messages
- Review this README thoroughly

## 🎓 Credits

Built with:
- [Chart.js](https://www.chartjs.org/) - Data visualization
- [Tailwind CSS](https://tailwindcss.com/) - Styling framework
- [PapaCSV](https://www.papaparse.com/) - CSV parsing
- [SheetJS](https://sheetjs.com/) - Excel processing
- [Google Gemini AI](https://ai.google.dev/) - AI insights
- [Lucide Icons](https://lucide.dev/) - Beautiful icons

---

**Made with ❤️ for smarter household financial management**