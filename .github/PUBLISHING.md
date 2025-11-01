# Publishing the Extension

This repository uses GitHub Actions to automatically publish the extension to the Visual Studio Code Marketplace when a new release is created.

## Setup Instructions

### 1. Create a Personal Access Token (PAT)

You need to create a Personal Access Token from Azure DevOps to publish to the VS Code Marketplace:

1. Go to https://dev.azure.com/
2. Click on "User Settings" > "Personal access tokens"
3. Click "New Token"
4. Set the following:
   - **Name**: VSCode Marketplace Publisher
   - **Organization**: All accessible organizations
   - **Expiration**: Choose your preferred duration
   - **Scopes**: Select "Marketplace" > "Manage"
5. Click "Create" and copy the token

### 2. Add the Token to GitHub Secrets

1. Go to your GitHub repository
2. Click on "Settings" > "Secrets and variables" > "Actions"
3. Click "New repository secret"
4. Set:
   - **Name**: `VSCE_PAT`
   - **Value**: Paste the token you created in step 1
5. Click "Add secret"

### 3. Create a Release

To publish a new version of the extension:

1. Update the version in `package.json`
2. Commit and push your changes
3. Go to your GitHub repository
4. Click on "Releases" > "Create a new release"
5. Create a new tag (e.g., `v1.0.1`)
6. Fill in the release title and description
7. Click "Publish release"

The GitHub Action will automatically:
- Check out the code
- Set up Node.js
- Install `vsce` (Visual Studio Code Extension Manager)
- Publish the extension to the marketplace

## Manual Publishing (Optional)

If you need to publish manually:

```bash
# Install vsce
npm install -g @vscode/vsce

# Package the extension
vsce package

# Publish the extension
vsce publish -p YOUR_PAT_TOKEN
```

## Troubleshooting

- **"Publisher not found"**: Make sure the publisher name in `package.json` matches your marketplace publisher
- **"Authentication failed"**: Check that your PAT token is valid and has the correct permissions
- **"Version already exists"**: Update the version number in `package.json`
