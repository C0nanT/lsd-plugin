# Setup

## GitHub Actions permissions

For `semantic-release` to create releases and commit the updated `CHANGELOG.md` back to the repo, the `GITHUB_TOKEN` needs write access.

**Steps:**

1. Go to your repository on GitHub
2. Navigate to **Settings → Actions → General**
3. Scroll to **Workflow permissions**
4. Select **Read and write permissions**
5. Click **Save**

## Using a Personal Access Token (optional)

If you prefer to use a PAT instead of the default `GITHUB_TOKEN`:

1. Create a fine-grained or classic PAT with `repo` scope
2. Go to **Settings → Secrets and variables → Actions**
3. Add a new secret named `GH_TOKEN` with your PAT value
4. Update `.github/workflows/release.yml` to use `GH_TOKEN: ${{ secrets.GH_TOKEN }}` instead of `GITHUB_TOKEN`
