# TODO Finder GitHub Action

A simple but powerful GitHub Action that automatically scans your codebase for `//TODO` comments and reports their count and locations whenever code is merged.

## Overview

As codebases grow, it's common to leave `//TODO` comments to mark areas that need future attention. However, these markers can easily be forgotten. This action helps you keep track of your technical debt by:

1. Counting all `//TODO` comments in your codebase
2. Listing their exact locations (file, line number, and content)
3. Providing this information as both a workflow output and a downloadable report
4. Commenting the results on merged PRs

## Installation

1. Create a `.github/workflows` directory in your repository (if it doesn't exist already)
2. Create a new file `.github/workflows/todo-finder.yml`
3. Copy the action YAML into this file
4. Commit and push to your repository

That's it! The action will now run automatically whenever code is merged to your main branch.

## When Does It Run?

The action is triggered on:
- Direct pushes to the main/master branch
- When a pull request to main/master is merged

It will not run on PRs that are closed without merging.

## Output

The action provides:

1. **GitHub Actions Workflow Summary**: The total number of TODOs appears in the workflow run summary.
2. **Downloadable Report**: A Markdown file with detailed information about TODO locations.
3. **PR Comments**: If triggered by a merged PR, the action will comment on that PR with the results.

Example report:
```markdown
# TODO Finder Results
Generated on: Mon Apr 04 15:23:45 UTC 2025

## Total TODOs found: 7

## TODO Locations:

- **src/components/UserProfile.js:42** - `//TODO: Add input validation`
- **src/utils/api.js:87** - `//TODO: Implement error handling`
- **src/reducers/auth.js:123** - `//TODO: Refactor this logic`
- ...
```

## Customization

### File Types

By default, the action scans the following file types:
```
*.js, *.jsx, *.ts, *.tsx, *.java, *.py, *.rb, *.php, *.c, *.cpp, *.h, *.hpp, *.cs, *.go, *.swift, *.kt
```

To modify this list, edit the `--include` pattern in the `Find TODOs` step.

### Excluded Directories

By default, the action excludes the following directories:
```
node_modules, .git, build, dist, vendor
```

To modify this list, edit the `--exclude-dir` pattern in the `Find TODOs` step.

### TODO Format

This action searches for both `//TODO` (without space) and `// TODO` (with space) comment formats. 

If you want to include additional formats (like Python-style `# TODO` comments), you can modify the grep patterns by adding to the regex pattern:

```bash
grep -r --include="*.{js,jsx,ts,tsx,java,py,rb,php,c,cpp,h,hpp,cs,go,swift,kt}" -E "(//TODO|// TODO|# TODO)" --exclude-dir={node_modules,.git,build,dist,vendor} . | wc -l
```

## Troubleshooting

### Action Not Running

- Ensure the workflow file is in the correct location (`.github/workflows/todo-finder.yml`)
- Check that you're merging to the branch specified in the workflow (default: `main` or `master`)
- Verify that your repository has GitHub Actions enabled

### Missing TODOs

- Check if your TODO format matches what the action is searching for (default: `//TODO`)
- Ensure your file type is included in the search patterns
- Verify that the directory containing your TODOs isn't in the excluded list

## License

This project is available under the MIT License. Feel free to modify and adapt it to your needs.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

For any issues or suggestions, please open an issue in this repository.