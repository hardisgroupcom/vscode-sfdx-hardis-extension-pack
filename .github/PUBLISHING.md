# Publishing the Extension

This repository uses GitHub Actions to automatically publish the extension to both the Visual Studio Code Marketplace and Open VSX Registry when a new release is created.

## Setup Instructions

### 1. Create Personal Access Tokens (PAT)

#### For Visual Studio Code Marketplace

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

#### For Open VSX Registry

You need to create an access token from Open VSX to publish there:

1. Go to https://open-vsx.org/
2. Sign in or create an account
3. Go to your user settings (click your username in the top right)
4. Navigate to "Access Tokens"
5. Click "Generate New Token"
6. Give it a description (e.g., "GitHub Actions Publisher")
7. Copy the generated token

### 2. Add the Tokens to GitHub Secrets

1. Go to your GitHub repository
2. Click on "Settings" > "Secrets and variables" > "Actions"
3. Add two repository secrets:
   
   **First secret:**
   - Click "New repository secret"
   - **Name**: `VSCE_PAT`
   - **Value**: Paste the Azure DevOps token
   - Click "Add secret"
   
   **Second secret:**
   - Click "New repository secret"
   - **Name**: `OVSX_PAT`
   - **Value**: Paste the Open VSX token
   - Click "Add secret"

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
- Install `vsce` and `ovsx` (extension publishing tools)
- Publish the extension to VS Code Marketplace
- Publish the extension to Open VSX Registry

## Manual Publishing (Optional)

If you need to publish manually:

```bash
# Install publishing tools
npm install -g @vscode/vsce ovsx

# Package the extension
vsce package

# Publish to VS Code Marketplace
vsce publish -p YOUR_VSCE_PAT

# Publish to Open VSX Registry
ovsx publish -p YOUR_OVSX_PAT
```

## Troubleshooting

- **"Publisher not found"**: Make sure the publisher name in `package.json` matches your marketplace publisher
- **"Authentication failed"**: Check that your PAT tokens are valid and have the correct permissions
- **"Version already exists"**: Update the version number in `package.json`
- **Open VSX namespace issues**: Make sure you have claimed your namespace on Open VSX (go to your profile settings)
