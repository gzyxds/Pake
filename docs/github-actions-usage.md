# GitHub Actions Usage Guide

<div align="center">

**English** · [中文](github-actions-usage_CN.md)

</div>

Build Pake apps online without installing development tools locally.

## Quick Steps

1. [Fork this project](https://github.com/tw93/Pake/fork)
2. In your fork, open the Actions tab, select `使用 Pake CLI 构建应用`, fill in the form (its fields are listed under [Form Inputs](#form-inputs) and cover a subset of the [CLI options](cli-usage.md)), and click `Run Workflow`

   ![Actions Interface](https://raw.githubusercontent.com/tw93/static/main/pake/action.png)

3. A green checkmark means the build succeeded, click the workflow name and download your app from the `Artifacts` section

   ![Build Success](https://raw.githubusercontent.com/tw93/static/main/pake/action2.png)

The first run builds the dependency cache and takes about 10-15 minutes, later runs use the cache and take about 5 minutes, and the complete cache is 400-600MB.

## Form Inputs

| Field                                                                    | What it does                                                           | Default        |
| ------------------------------------------------------------------------ | ---------------------------------------------------------------------- | -------------- |
| `构建平台`                                                               | Runner to build on: `windows-latest`, `macos-latest` or `ubuntu-24.04` | `macos-latest` |
| `网站地址`                                                               | The site to package, required                                          | none           |
| `应用名称（Linux 下使用小写）`                                           | App name and artifact prefix, required                                 | none           |
| `图标地址（留空则自动获取）`                                             | Icon for the app, fetched from the site when empty                     | empty          |
| `窗口宽度（像素）`                                                       | Initial window width                                                   | `1200`         |
| `窗口高度（像素）`                                                       | Initial window height                                                  | `780`          |
| `最小窗口宽度（像素）`                                                   | Smallest width the window can shrink to                                | empty          |
| `最小窗口高度（像素）`                                                   | Smallest height the window can shrink to                               | empty          |
| `应用版本`                                                               | Version written into the package                                       | `1.0.0`        |
| `Windows 安装包语言（仅 Windows 生效）`                                  | Installer language for the MSI                                         | `zh-CN`        |
| `构建成功后发布到 GitHub Releases`                                       | Publish the installer as a GitHub Release                              | on             |
| `Release 标签（留空自动生成）`                                           | Release tag; defaults to `v<version>-<run number>`                     | empty          |
| `启动时进入全屏模式`                                                     | Open the app in fullscreen                                             | off            |
| `隐藏标题栏（仅 macOS）`                                                 | Hide the window title bar                                              | off            |
| `允许网站打开新窗口`                                                     | Let the site open pop-up windows                                       | off            |
| `通用二进制（仅 macOS）`                                                 | Build one app for Intel and Apple Silicon                              | off            |
| `Linux 安装包格式（仅 Linux 生效，用逗号分隔：deb、appimage、rpm、zst）` | Linux package formats, ignored on other platforms                      | `deb`          |

## Artifacts

Each run uploads the package as an artifact named after the app: `<name>-macOS` holds the `.dmg`, `<name>-Windows` the `.msi`, and Linux gets one artifact per format, `<name>-Linux-deb`, `<name>-Linux-AppImage`, `<name>-Linux-rpm` and `<name>-Linux-zst`. Artifacts are kept for 3 days, so download them soon after the build. Instead of the `Artifacts` section, you can also fetch them with the GitHub CLI: `gh run download RUN_ID`.

## Releases

When `构建成功后发布到 GitHub Releases` is enabled, the workflow creates or updates a GitHub Release for the repository and uploads the generated installer. Leave `Release 标签（留空自动生成）` empty to use `v<version>-<run number>`, or enter a stable tag such as `v1.0.0` to replace that release's installer on later runs.

## Tips

- Enable `允许网站打开新窗口` when the site launches sign-in, exam, or other flows in a separate window
- Use `zh-CN` for the Windows installer language when the app name contains Chinese characters
- Disable `构建成功后发布到 GitHub Releases` if you only want an Actions artifact
- If a build fails, clear the Actions cache and retry

## Related Docs

- [CLI Usage Guide](cli-usage.md)
- [Advanced Usage](advanced-usage.md)
