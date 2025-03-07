### Rule
``` yaml
rules:
  - id: pyramid-csrf-check-disabled-globally
    patterns:
      - pattern-inside: |
          $CONFIG.set_default_csrf_options(..., require_csrf=$REQUIRE_CSRF, ...)
      - pattern: $REQUIRE_CSRF
      - metavariable-comparison:
          metavariable: $REQUIRE_CSRF
          comparison: $REQUIRE_CSRF == False
    message: Automatic check of cross-site request forgery tokens has been
      explicitly disabled globally, which might leave views unprotected. Use
      'pyramid.config.Configurator.set_default_csrf_options(require_csrf=True)'
      to turn the automatic check for all unsafe methods (per RFC2616).
    languages:
      - python
    severity: ERROR
    fix: |
      True
    metadata:
      cwe:
        - "CWE-352: Cross-Site Request Forgery (CSRF)"
      owasp:
        - A01:2021 - Broken Access Control
      category: security
      technology:
        - pyramid
      references:
        - https://owasp.org/Top10/A01_2021-Broken_Access_Control
      cwe2022-top25: true
      cwe2021-top25: true
      subcategory:
        - vuln
      likelihood: LOW
      impact: LOW
      confidence: MEDIUM
      license: Commons Clause License Condition v1.0[LGPL-2.1-only]
      vulnerability_class:
        - Cross-Site Request Forgery (CSRF)
````
### Code Snippet
```python
from pyramid.csrf import CookieCSRFStoragePolicy


def includeme_bad(config):
    config.set_csrf_storage_policy(CookieCSRFStoragePolicy())
    # ruleid: pyramid-csrf-check-disabled-globally
    config.set_default_csrf_options(require_csrf=False)
```
### Transformation 1*
```python
from pyramid.csrf import CookieCSRFStoragePolicy


def includeme_bad(config):
    config.set_csrf_storage_policy(CookieCSRFStoragePolicy())
    # ruleid: pyramid-csrf-check-disabled-globally
    fator = (False, True)
    config.set_default_csrf_options(require_csrf=fator[0])
```


### Transformation 2
```python
from pyramid.csrf import CookieCSRFStoragePolicy


def includeme_bad(config):
    config.set_csrf_storage_policy(CookieCSRFStoragePolicy())
    # ruleid: pyramid-csrf-check-disabled-globally
    def check_origin():
        return False
    config.set_default_csrf_options(require_csrf=check_orgin())
```
### Transformation 3
```python
from pyramid.csrf import CookieCSRFStoragePolicy

def includeme_bad(config):
    config.set_csrf_storage_policy(CookieCSRFStoragePolicy())

    # Evading ruleid: pyramid-csrf-check-disabled-globally
    def set_options(check_origin=False):
        config.set_default_csrf_options(check_origin=check_origin)
    set_options()
```
