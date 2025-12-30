# Contributing to Data Classification & Incident Response System

Thank you for your interest in contributing! This document provides guidelines for submitting improvements, fixes, and enhancements.

## Code of Conduct

This project is committed to providing a welcoming and inspiring community for all. Please be respectful and constructive in all interactions.

## How to Contribute

### Reporting Issues

1. **Check existing issues** - Search to avoid duplicates
2. **Be specific** - Describe what you expected vs. what happened
3. **Include context** - Relevant logs, configuration, or examples
4. **Security issues** - Email security@yourcompany.com instead of public issues

### Making Changes

1. **Fork the repository**
   ```bash
   git clone https://github.com/YOUR-USERNAME/data-classification-incident-response.git
   cd data-classification-incident-response
   ```

2. **Create a branch**
   ```bash
   git checkout -b feature/my-improvement
   # or
   git checkout -b fix/my-bugfix
   ```

3. **Make your changes**
   - Follow PEP 8 for Python code
   - Update documentation as needed
   - Add tests for new functionality
   - Keep commits focused and descriptive

4. **Test your changes**
   ```bash
   pip install -r requirements.txt
   pytest
   black .
   flake8 .
   ```

5. **Push to your fork**
   ```bash
   git push origin feature/my-improvement
   ```

6. **Create a Pull Request**
   - Clear title and description
   - Reference related issues
   - Describe testing done
   - Link to relevant documentation

## Contribution Areas

### Documentation
- Clarify existing docs
- Add examples
- Improve playbooks
- Translate to other languages

### Code
- Bug fixes
- New tools and utilities
- Performance improvements
- Test coverage

### Playbooks
- New incident types
- Additional containment steps
- Detection rules
- Recovery procedures

### Configuration
- Security rules
- Detection signatures
- Template improvements
- Tool integrations

## Style Guidelines

### Python Code
- PEP 8 compliant
- Type hints where practical
- Docstrings for functions and classes
- Meaningful variable names
- Max 100 character line length

### Documentation
- Clear, technical writing
- Code blocks with syntax highlighting
- Examples and real-world scenarios
- Cross-references to related docs

### Commit Messages
```
type(scope): Brief description

More detailed explanation if needed.
Multiple paragraphs are fine.

Fixes #123
```

Types: feat, fix, docs, style, refactor, perf, test, chore

## Review Process

1. **Automated checks** - All tests and linting must pass
2. **Code review** - At least one maintainer review required
3. **Feedback** - Address any requested changes
4. **Approval** - Once approved, PR can be merged

## Pull Request Process

- ✅ Automated tests pass
- ✅ Code review complete
- ✅ Documentation updated
- ✅ No merge conflicts
- ✅ Commit history is clean

## Release Process

Releases follow semantic versioning (MAJOR.MINOR.PATCH).
Maintainers tag releases and update CHANGELOG.md.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

## Questions?

- Open a discussion
- Check existing documentation
- Email maintainers for security issues

## Appreciation

Thank you for helping make this project better!
