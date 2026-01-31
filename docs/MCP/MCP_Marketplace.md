# Extension Marketplace Guide

**Finding and Publishing Extensions**

---

## Finding Extensions

### Official Extensions

**Development Tools:**

- `ollm-dev-tools` - Code formatting, linting, testing
- `ollm-git` - Git integration and workflows

**Integrations:**

- `ollm-github` - GitHub API integration
- `ollm-gitlab` - GitLab API integration
- `ollm-database` - Database tools (PostgreSQL, MySQL, SQLite)

**Documentation:**

- `ollm-docs` - Documentation templates and generators

### Community Extensions

Browse community extensions:

- GitHub Topics (https://github.com/topics/ollm-extension)
- Extension Registry (https://github.com/ollm/extensions)

### Search Extensions

```bash
# Search by keyword
/extensions search github

# Search by category
/extensions search --category integration
```

---

## Installing Extensions

### From Registry

```bash
# Install by name
/extensions install github-integration

# Install specific version
/extensions install github-integration@1.2.0
```

### From URL

```bash
# Install from GitHub release
/extensions install https://github.com/user/ext/releases/download/v1.0.0/ext.tar.gz
```

### From File

```bash
# Install local file
/extensions install ./my-extension.tar.gz
```

---

## Publishing Extensions

### 1. Prepare Extension

- Create manifest.json
- Add README.md
- Add LICENSE
- Test thoroughly
- Package as .tar.gz

### 2. Create GitHub Repository

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/user/my-extension
git push -u origin main
```

### 3. Create Release

```bash
# Tag version
git tag v1.0.0
git push --tags

# Create release with archive
gh release create v1.0.0 my-extension-1.0.0.tar.gz
```

### 4. Submit to Registry

1. Fork ollm/extensions (https://github.com/ollm/extensions)
2. Add your extension to registry.json
3. Submit pull request
4. Wait for review

---

## Extension Guidelines

### Quality Standards

**Required:**

- ✅ Valid manifest.json
- ✅ README.md with usage instructions
- ✅ LICENSE file
- ✅ Semantic versioning
- ✅ Working examples

**Recommended:**

- ✅ Comprehensive documentation
- ✅ Test suite
- ✅ Changelog
- ✅ Contributing guidelines

### Security Requirements

**Must:**

- ✅ Request minimal permissions
- ✅ No malicious code
- ✅ No data collection without disclosure
- ✅ Safe dependencies

**Must Not:**

- ❌ Include credentials or secrets
- ❌ Execute arbitrary code
- ❌ Access sensitive files
- ❌ Make undisclosed network requests

---

## Further Reading

- [Extension System Overview](3%20projects/OLLM%20CLI/Extensions/README.md)
- [Extension User Guide](3%20projects/OLLM%20CLI/Extensions/user-guide.md)
- [Extension Development Guide](3%20projects/OLLM%20CLI/Extensions/development-guide.md)
- [Manifest Reference](manifest-reference.md)

---

**Last Updated:** 2026-01-16  
**Version:** 0.1.6

<!-- Alpha release v0.1.6 -->

