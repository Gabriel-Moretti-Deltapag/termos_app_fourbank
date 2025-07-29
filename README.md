# DeltaPag Terms of Use - Static Website

This repository contains the Terms of Use website for DeltaPag Soluções de Pagamento, designed as a static website with automated deployment to AWS S3 and CloudFront.

## 🌟 Features

- **Two Versions**: Premium styled version (`index.html`) and basic clean version (`basic.html`)
- **Responsive Design**: Works perfectly on desktop, tablet, and mobile devices
- **Professional Styling**: Company branding with orange, black, and white color scheme
- **LGPD Compliant**: Includes comprehensive data protection and privacy sections
- **Automated Deployment**: GitHub Actions CI/CD pipeline
- **AWS Infrastructure**: S3 static hosting with CloudFront CDN
- **Security**: WAF protection and rate limiting
- **Performance**: Optimized caching and compression

## 📁 Repository Structure

```bash
├── index.html                    # Premium styled version
├── basic.html                    # Clean, basic version
├── assets/                       # Static assets
│   ├── favicon-16x16.png
│   ├── favicon-32x32.png
│   ├── favicon.ico
│   ├── apple-touch-icon.png
│   ├── android-chrome-192x192.png
│   ├── android-chrome-512x512.png
│   ├── logopng.png              # Company logo
│   └── site.webmanifest
├── .github/workflows/
│   └── deploy.yml               # GitHub Actions workflow
├── infrastructure/
│   └── cloudformation.yml       # AWS infrastructure template
└── README.md                    # This file
```

## 🚀 Quick Start

### Prerequisites

- AWS Account with appropriate permissions
- Git repository on GitHub

### GitHub Actions Setup

1. **Fork/Clone this repository**

2. **Configure GitHub Secrets:**

   Go to your repository → Settings → Secrets and Variables → Actions

   **Required Secrets:**

   ```bash
   AWS_ACCESS_KEY_ID=your_aws_access_key
   AWS_SECRET_ACCESS_KEY=your_aws_secret_key
   ```

3. **Configure GitHub Variables (Optional):**

   ```bash
   AWS_REGION=us-east-1                    # Default: us-east-1
   S3_BUCKET_NAME=deltapag-termos-site     # Default: deltapag-termos-site
   ENVIRONMENT=prod                        # Default: prod
   ```

4. **Deploy:**

   ```bash
   git push origin main
   ```

   The GitHub Actions workflow will automatically:
   - Deploy the CloudFormation infrastructure
   - Upload files to S3
   - Invalidate CloudFront cache
   - Provide the website URL

## 🏗️ Infrastructure

The CloudFormation template creates:

### Core Infrastructure

- **S3 Bucket**: Public website hosting in `sa-east-1` region (Brazilian compliance)
- **CloudFront Distribution**: Global CDN with HTTPS (deployed from `us-east-1`)

### Security & Monitoring

- **Public S3 Bucket**: Simplified setup, direct access allowed
- **HTTPS via CloudFront**: Secure delivery globally
- **Cost Optimized**: Minimal AWS services for maximum savings

### Performance Optimization

- **Caching Strategy**:
  - HTML files: 1 day cache
  - Assets: 1 year cache
  - Compression enabled
- **Error Pages**: Fallback to `basic.html` for 404/403 errors

## 🔧 Configuration Options

### CloudFormation Parameters

| Parameter | Description | Default | Options |
|-----------|-------------|---------|---------|
| `BucketName` | S3 bucket name prefix | `deltapag-termos-site` | Any valid S3 name |
| `Environment` | Environment name | `prod` | `dev`, `staging`, `prod` |
| `S3BucketName` | Full S3 bucket name | Auto-generated | Generated from prefix |
| `S3Region` | S3 bucket region | `sa-east-1` | AWS region code |

### GitHub Actions Variables

Set these in your repository's Variables section:

- `S3_BUCKET_NAME`: Bucket name prefix (optional)
- `ENVIRONMENT`: Environment identifier (optional)

**Note**: CloudFront resources deployed to `us-east-1` (required), S3 bucket created in `sa-east-1` (company compliance).

**Security Note**: This setup uses a public S3 bucket for simplicity. Users can access files directly via S3 URLs, bypassing CloudFront. For higher security, consider using Origin Access Identity (OAI).

## 📝 Content Management

### Updating Terms

1. **Edit the HTML files:**
   - `index.html`: Premium version with full styling
   - `basic.html`: Clean version for printing/accessibility

2. **Commit and push:**

   ```bash
   git add .
   git commit -m "Update terms of use"
   git push origin main
   ```

3. **Automatic deployment:**
   - GitHub Actions will automatically deploy changes
   - CloudFront cache will be invalidated
   - Website will be updated within minutes

### Adding New Sections

Follow the existing HTML structure and CSS classes:

```html
<section id="new-section" class="section">
    <h2 class="section-title">NEW SECTION TITLE</h2>
    
    <div class="subsection">
        <h3>Subsection Title</h3>
        <div class="highlight">
            <p>Content with highlight styling</p>
        </div>
    </div>
</section>
```

## 🎨 Styling Guide

### CSS Classes Available

- `.section`: Main section container
- `.section-title`: Section headings
- `.subsection`: Subsection container
- `.definition`: Definition boxes
- `.important`: Important notices (yellow background)
- `.warning`: Warning notices (red/black background)
- `.highlight`: Highlighted content (green background)
- `.contact-info`: Contact information box

### Company Colors

- **Primary Orange**: `#ff6b35`
- **Secondary Orange**: `#ff8500`
- **Accent Gold**: `#f39c12`
- **Text Black**: `#000000`
- **Background White**: `#ffffff`

## 🔒 Security Features

### SSL/TLS

- HTTPS via CloudFront default certificate
- Automatic HTTP to HTTPS redirection
- Secure content delivery

### Access Control

- Public read-only access to website content
- S3 bucket configured for public website hosting
- CloudFront provides additional global access layer

## 📊 Monitoring & Alerting

### Basic Monitoring

- AWS Console access to CloudFront metrics
- S3 access logs (if needed)
- GitHub Actions deployment logs
- Cost-optimized approach with minimal monitoring overhead

## 🧪 Testing

### Automated Tests

The deployment pipeline includes:

- CloudFormation template validation
- HTTP response code verification
- SSL certificate validation

### Manual Testing

```bash
# Test website accessibility
curl -I https://your-website-url.com

# Test both versions
curl -I https://your-website-url.com/index.html
curl -I https://your-website-url.com/basic.html

# Test assets
curl -I https://your-website-url.com/assets/favicon-32x32.png
```

## 🚨 Troubleshooting

### Common Issues

#### 1. CloudFormation Deploy Fails

```bash
# Check stack events
aws cloudformation describe-stack-events --stack-name deltapag-termos-stack

# Validate template locally
aws cloudformation validate-template --template-body file://infrastructure/cloudformation.yml
```

#### 2. Files Not Updating

```bash
# Manually invalidate CloudFront
aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```

#### 3. GitHub Actions Fails

- Check AWS credentials in repository secrets
- Verify IAM permissions for CloudFormation and S3
- Check CloudFormation logs in AWS Console

### Getting Help

1. Check AWS CloudFormation console for stack events
2. Review GitHub Actions logs
3. Verify IAM permissions include:
   - CloudFormation full access
   - S3 full access
   - CloudFront full access

## 💰 Cost Estimation

**Monthly costs (estimated):**

- S3 Storage: ~$0.10 (for small static site)
- CloudFront: **FREE** (first 1TB transfer + 10M requests/month)
- Data transfer: **FREE** (within free tier limits)
- **Total: ~$0.10/month** 🎉

*Most small to medium websites will operate entirely within AWS Free Tier limits, making this virtually free!*

**Important**: Users can access files directly from S3, potentially bypassing CloudFront free tier. Monitor usage to ensure you stay within free tier limits.

### AWS Free Tier Benefits

- **CloudFront**: 1TB data transfer out + 10M HTTP/HTTPS requests
- **S3**: 5GB storage + 20,000 GET requests + 2,000 PUT requests
- **Lambda**: Not used (cost savings)
- **Route53**: Not used (cost savings)
- **WAF**: Not used (cost savings)

## 📋 Maintenance

### Regular Tasks

- Update content as needed
- Monitor AWS costs (should be minimal!)
- Review GitHub Actions workflow

### Updates

- Keep dependencies updated in GitHub Actions
- SSL certificates managed automatically by CloudFront
- No manual infrastructure maintenance required

## 📄 License

This project is proprietary to DeltaPag Soluções de Pagamento LTDA.

## 🆘 Support

For technical support:

- **GitHub Issues**: Create an issue in this repository
- **Email**: Contact the development team
- **Documentation**: Refer to AWS documentation for infrastructure issues

---

**🚀 Ready to deploy?** Follow the Quick Start guide above!

**📖 Need help?** Check the Troubleshooting section or create an issue.
