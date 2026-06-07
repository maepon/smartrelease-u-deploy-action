# smartrelease-u-deploy-action

GitHub Action to deploy to SmartRelease U test server via SFTP.
Under the hood, it uses `wlixcc/SFTP-Deploy-Action` with optimized settings for the SmartRelease U environment.

## Features

- Uses SFTP only (mandated by SmartRelease U).
- Pre-configured optimized connection timeout (`-o ConnectTimeout=5`).
- Optional size-only file comparison (`--size-only`).

## Inputs

| Input name | Description | Required | Default |
| :--- | :--- | :--- | :--- |
| `local_path` | Local directory to upload (e.g., `./public/*`) | **Yes** | - |
| `remote_path` | Remote path on the SFTP server (e.g., `/www`) | **Yes** | - |
| `sftp_server` | SFTP server host address | No | `sftp.pre-svr.jp` |
| `sftp_username` | SFTP username | **Yes** | - |
| `sftp_private_key` | SSH private key corresponding to the SFTP user | **Yes** | - |
| `sftp_port` | SFTP port | No | `22` |
| `size_only` | ファイルサイズのみで比較してアップロードするかどうか | No | `false` |

## Usage Example

Add the following step to your GitHub Actions workflow file (e.g., `.github/workflows/deploy.yml`):

```yaml
name: Deploy to SmartRelease U

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      # Add your build steps here if needed (e.g., npm run build)

      - name: Deploy to SmartRelease U
        uses: maepon/smartrelease-u-deploy-action@v1
        with:
          local_path: './dist/*'
          remote_path: '/path/to/remote/dir'
          sftp_server: ${{ secrets.SFTP_SERVER }}
          sftp_username: ${{ secrets.SFTP_USERNAME }}
          sftp_private_key: ${{ secrets.SFTP_PRIVATE_KEY }}
          sftp_port: '22'
          size_only: 'false' # サイズ比較のみで転送判定を行う場合は 'true' に指定します

```