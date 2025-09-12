# OpenDTU-OnBattery Documentation

OpenDTU-OnBattery documentation is a MkDocs-based static documentation website for ESP32 firmware that communicates with Hoymiles solar inverters and battery management systems. It's built using Python, Material for MkDocs theme, and deployed automatically to GitHub Pages.

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Bootstrap the repository:
- `python3 -m venv venv` -- creates virtual environment in 3 seconds
- `source venv/bin/activate` -- activates the virtual environment
- `pip install --upgrade pip wheel setuptools` -- upgrades package tools in 4 seconds
- `pip install -r requirements.txt` -- installs all MkDocs dependencies in 27 seconds

### Build and test the documentation:
- `mkdocs build` -- builds static site to site/ directory in 4-5 seconds. FAST.
- `mkdocs build --strict` -- builds with strict validation, same timing as regular build
- `mkdocs build --clean --strict` -- builds with strict validation and clean site directory
- `mkdocs serve` -- starts development server on http://127.0.0.1:8000/ with live reload
- Live reload rebuilds in 4-5 seconds when files change

## Validation

### Manual validation scenarios:
- ALWAYS test the development server after making documentation changes: `mkdocs serve` then visit http://127.0.0.1:8000/
- ALWAYS run `mkdocs build --strict` to catch validation errors before committing
- Test navigation by checking that new pages appear in the site navigation correctly
- For content changes, verify markdown rendering by viewing the affected pages in the browser
- For configuration changes in mkdocs.yml, restart the development server to see effects
- **End-to-end validation**: After changes, run through a complete scenario: build, serve, navigate to changed pages, verify content renders correctly

### Development workflow:
- Always bootstrap the environment first using the commands above
- Use `mkdocs serve` for live development with automatic rebuilds
- Test builds with `mkdocs build --strict` before committing changes
- The CI pipeline (.github/workflows/build.yaml) will build and deploy to GitHub Pages on main branch pushes

## Repository Structure

### Key directories and files:
```
/docs/                  # All documentation content (73+ markdown files)
  ├── index.md         # Homepage/introduction
  ├── firmware/        # Firmware-related documentation
  ├── hardware/        # Hardware documentation
  ├── 3rd_party/       # Third-party integrations
  └── assets/          # Images, CSS, JavaScript
/mkdocs.yml            # MkDocs configuration (navigation, theme, plugins)
/requirements.txt      # Python dependencies (MkDocs + plugins)
/.github/workflows/    # CI/CD pipeline for GitHub Pages deployment
/site/                 # Generated static site (gitignored)
/venv/                 # Python virtual environment (gitignored)
```

### Common file locations:
- **Navigation configuration**: mkdocs.yml (nav section defines site structure)
- **Homepage content**: docs/index.md
- **Images and assets**: docs/assets/
- **Theme configuration**: mkdocs.yml (theme section)
- **Plugin configuration**: mkdocs.yml (plugins section)

## Common Tasks

### Adding new documentation:
1. Create new .md file in appropriate docs/ subdirectory
2. Add entry to navigation in mkdocs.yml (nav section)
3. Test with `mkdocs serve` to verify navigation and content
4. Run `mkdocs build --strict` to validate

### Modifying existing content:
1. Edit the .md file directly
2. Test changes with `mkdocs serve` (live reload active)
3. Verify content renders correctly in browser
4. Run `mkdocs build --strict` to validate

### Updating configuration:
1. Edit mkdocs.yml
2. Restart development server (`mkdocs serve`)
3. Test affected functionality
4. Run `mkdocs build --strict` to validate configuration

### Troubleshooting builds:
- Check mkdocs.yml syntax if build fails
- Verify all files referenced in navigation exist
- Use `mkdocs build --verbose` for detailed build information
- Use `mkdocs build --strict` to catch warnings as errors

## CI/CD Pipeline

The GitHub Actions workflow (.github/workflows/build.yaml) automatically:
1. Sets up Python 3.x environment
2. Installs dependencies from requirements.txt
3. Runs `mkdocs build`
4. Deploys to GitHub Pages on main branch (using `mkdocs gh-deploy --force`)

### Deployment process:
- Push to main branch triggers automatic build and deployment
- Site deploys to GitHub Pages at https://opendtu-onbattery.net/
- No manual deployment steps required

## Time Expectations

All operations are very fast for this documentation site:
- Virtual environment setup: 3 seconds
- Dependency installation: 27 seconds (first time only)
- Build time: 4-5 seconds
- Live reload rebuild: 4-5 seconds
- Development server startup: 4-5 seconds

## Dependencies and Requirements

### System requirements:
- Python 3.x (tested with Python 3.12.3)
- pip package manager
- Git for version control

### Python packages (from requirements.txt):
- mkdocs (core static site generator)
- mkdocs-material (Material Design theme)
- mkdocs-git-revision-date-localized-plugin (git revision dates)
- mkdocs-glightbox (image lightbox)
- mkdocs-macros-plugin (templating and macros)
- mkdocs-swagger-ui-tag (API documentation)
- pymdown-extensions (enhanced markdown features)

### Installation verification:
Run `mkdocs get-deps` to see required core dependencies that should match requirements.txt.

## Known Issues and Warnings

### Expected warnings during build:
- `DeprecationWarning: Call to deprecated method replaceWith` from mkdocs-swagger-ui-tag plugin - does not affect functionality
- `No default module 'main' found` from macros plugin - expected when no main.py exists
- Pages not in navigation warning for 3rd_party/opendtu_fusion_endorsement.md - intentional

### Network requirements:
- Initial pip install commands require internet access to PyPI (pypi.org)
- If pip commands fail due to network timeouts, wait and retry - this is a known limitation in some environments
- Once dependencies are installed, all build and serve commands work offline

### Important notes:
- The site/ directory is automatically generated and should not be edited manually
- Virtual environment (venv/) should not be committed to Git
- Always use the virtual environment when working with the documentation
- GitHub Pages deployment requires the CNAME file in docs/ for custom domain