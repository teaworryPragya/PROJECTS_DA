# Blinkit Quick Commerce Analytics Dashboard

A comprehensive full-stack analytics dashboard for Blinkit's quick commerce platform featuring real-time insights, customer analytics, product performance, and delivery metrics.

## 🚀 Features

### Dashboard Tabs
- **Overview Dashboard**: KPIs, orders/revenue trends, payment methods
- **Customer Insights**: Demographics, location heatmaps, RFM segmentation
- **Product Analytics**: Top products, category sales, inventory management
- **Delivery Performance**: Delivery times, route optimization, performance metrics
- **Revenue & Growth**: Revenue trends, profit margins, growth forecasting

### Tech Stack
- **Frontend**: React + Tailwind CSS + Recharts
- **Backend**: Node.js + Express
- **Deployment**: Vercel (Frontend) + Render (Backend)

## 📁 Project Structure

```
blinkit-analytics-dashboard/
├── backend/                 # Node.js + Express API
│   ├── src/
│   │   ├── controllers/
│   │   ├── routes/
│   │   ├── data/
│   │   └── utils/
│   ├── package.json
│   └── server.js
├── frontend/               # React + Tailwind Dashboard
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/
│   │   └── utils/
│   ├── package.json
│   └── public/
└── README.md
```

## 🛠️ Setup Instructions

### Backend Setup
```bash
cd backend
npm install
npm run dev
```

### Frontend Setup
```bash
cd frontend
npm install
npm start
```

## 🌐 Deployment

- **Frontend**: Deployed on Vercel
- **Backend**: Deployed on Render

## 📊 Mock Data

The project includes comprehensive mock datasets for:
- Customer demographics and behavior
- Order history and transactions  
- Product catalog and inventory
- Delivery performance metrics

## 🎨 UI Features

- Dark/Light mode toggle
- Responsive design (desktop + mobile)
- Interactive charts and visualizations
- Export to CSV functionality
- Smooth animations and transitions
