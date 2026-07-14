issue: sourcing oe-init-build-env being affected by jupiter
unset TEMPLATECONF
. oe-init-build-env



---
## Tasks:
### 1. Remove sstate and download paths from local.conf and make them abstract
### 2.  