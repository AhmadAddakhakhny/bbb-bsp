issue: sourcing oe-init-build-env being affected by jupiter
unset TEMPLATECONF
. oe-init-build-env



---
## Tasks:
### 1. Remove sstate and download paths from local.conf and make them abstract
### 2.  create a oe-init-build-env on a project level
### 3. project naming convention to suite several boards
### 4. abstract way to append the layers
### 5. define different machines for build