# Power BI Integration Setup Guide

This guide will help you configure Power BI integration for the Blinkit Analytics Dashboard.

## Prerequisites

1. **Power BI Pro or Premium License**
2. **Azure Active Directory Access**
3. **Power BI Workspace with Reports**

## Step 1: Azure AD App Registration

### 1.1 Create Azure AD Application
1. Go to [Azure Portal](https://portal.azure.com)
2. Navigate to **Azure Active Directory** > **App registrations**
3. Click **New registration**
4. Fill in the details:
   - **Name**: `Blinkit Analytics Dashboard`
   - **Supported account types**: Accounts in this organizational directory only
   - **Redirect URI**: `http://localhost:3000` (for development)

### 1.2 Configure API Permissions
1. In your app registration, go to **API permissions**
2. Click **Add a permission** > **Power BI Service**
3. Add these **Delegated permissions**:
   - `Report.Read.All`
   - `Dataset.Read.All`
   - `Workspace.Read.All`
4. Click **Grant admin consent**

### 1.3 Create Client Secret
1. Go to **Certificates & secrets**
2. Click **New client secret**
3. Add description and set expiration
4. **Copy the secret value** (you won't see it again)

### 1.4 Note Required IDs
Copy these values for your environment variables:
- **Application (client) ID**
- **Directory (tenant) ID**
- **Client secret value**

## Step 2: Power BI Workspace Setup

### 2.1 Create Workspace
1. Go to [Power BI Service](https://app.powerbi.com)
2. Create a new workspace or use existing one
3. Note the **Workspace ID** from the URL: `app.powerbi.com/groups/{workspace-id}/`

### 2.2 Upload Reports
1. Upload your `.pbix` files to the workspace
2. For each report, note the **Report ID** from the URL when viewing the report
3. The embed URL format: `https://app.powerbi.com/reportEmbed?reportId={report-id}`

### 2.3 Configure Workspace Access
1. In workspace settings, add your Azure AD app as a **Member** or **Admin**
2. Ensure the service principal has access to embed reports

## Step 3: Environment Configuration

### 3.1 Copy Environment Template
```bash
cp frontend/.env.example frontend/.env
```

### 3.2 Fill in Your Values
Edit `frontend/.env` with your actual values:

```bash
# Required - Azure AD Configuration
REACT_APP_POWERBI_CLIENT_ID=your_client_id_here
REACT_APP_POWERBI_CLIENT_SECRET=your_client_secret_here
REACT_APP_POWERBI_TENANT_ID=your_tenant_id_here
REACT_APP_POWERBI_WORKSPACE_ID=your_workspace_id_here

# Required - Access Token (see Step 4 for generation)
REACT_APP_POWERBI_ACCESS_TOKEN=your_access_token_here

# Optional - Report IDs (configure as needed)
REACT_APP_POWERBI_OVERVIEW_REPORT_ID=your_overview_report_id
REACT_APP_POWERBI_SALES_REPORT_ID=your_sales_report_id
REACT_APP_POWERBI_CUSTOMERS_REPORT_ID=your_customers_report_id
REACT_APP_POWERBI_PRODUCTS_REPORT_ID=your_products_report_id
REACT_APP_POWERBI_DELIVERY_REPORT_ID=your_delivery_report_id
```

## Step 4: Generate Access Token

You have several options for authentication:

### Option A: Service Principal (Recommended for Production)
Use Azure AD service principal authentication. This requires backend implementation for token management.

### Option B: User Token (Development)
1. Use [Power BI REST API](https://docs.microsoft.com/en-us/rest/api/power-bi/)
2. Generate token using your Azure AD credentials
3. Token expires after 1 hour and needs refresh

### Option C: Embed Token (Most Secure)
Implement backend endpoint to generate embed tokens for specific reports.

## Step 5: Install Dependencies

```bash
cd frontend
npm install powerbi-client powerbi-client-react
```

## Step 6: Test Integration

1. Start your application:
```bash
npm start
```

2. Navigate to **Power BI Reports** in the dashboard
3. If configuration is missing, you'll see the setup guide
4. Use the **Configuration** section to validate your settings

## Troubleshooting

### Common Issues

1. **"Report not found" Error**
   - Verify report ID is correct
   - Check workspace access permissions
   - Ensure report is published and accessible

2. **Authentication Errors**
   - Verify Azure AD app permissions
   - Check client ID and tenant ID
   - Ensure admin consent is granted

3. **Token Expiration**
   - Implement token refresh mechanism
   - Use service principal for long-running applications
   - Consider embed tokens for better security

4. **CORS Issues**
   - Ensure redirect URIs are configured in Azure AD
   - Check Power BI workspace settings
   - Verify embed URLs are correct

### Security Best Practices

1. **Never expose client secrets** in frontend code
2. **Use environment variables** for all sensitive data
3. **Implement token refresh** for production use
4. **Use HTTPS** in production environments
5. **Regularly rotate** client secrets and tokens

## Additional Resources

- [Power BI Embedded Documentation](https://docs.microsoft.com/en-us/power-bi/developer/embedded/)
- [Power BI REST API Reference](https://docs.microsoft.com/en-us/rest/api/power-bi/)
- [Azure AD App Registration Guide](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app)

## Support

For issues with Power BI integration:
1. Check the browser console for detailed error messages
2. Verify all environment variables are set correctly
3. Test API permissions in Azure AD
4. Consult Power BI embedding documentation
