# GitHub Catalogue
## Software

* **[rcowsill/workflow-scan-action](https://github.com/rcowsill/workflow-scan-action)** (View on [Actions Marketplace](https://github.com/marketplace/actions/workflow-scan-action))
  - A GitHub Action which scans GitHub Actions workflow files using CodeQL.
  - This makes it easy to use the [CodeQL queries](https://github.com/github/codeql/tree/main/javascript/ql/src/experimental/Security/CWE-094)
    created by the GitHub Security lab on the workflows in your repo.
* **[rcowsill/SO-Focused](https://github.com/rcowsill/SO-Focused)** (View on [addons.mozilla.org](https://addons.mozilla.org/en-GB/addon/so-focused/))
  - A CSS-only WebExtension which hides the distracting sidebar elements on StackOverflow and related sites.

## Electronics

* **[rcowsill/USCI40_demo](https://github.com/rcowsill/USCI40_demo): MSP430 USCI40 erratum demonstration**
  - A test firmware written to characterise a bug in the MSP430 USCI peripheral, and a report on the results.
  - The results were passed on to Texas Instruments, leading to the addition of USCI40 to the errata list.
* **[MSP430 variable delay routine](https://gist.github.com/rcowsill/213b8db204a554c419369d1027b3e221)** (Gist)
  - Used in the USCI40 test firmware.
* **[rcowsill/TestCards](https://github.com/rcowsill/TestCards)**
  - A test image that shows whether your graphics card's RGB output range matches your monitor's input range.
  - Incorrect settings will make the display either clip shadows and highlights, or appear washed out.


---

## Contributions
### CI/CD

* **[actions/toolkit#792](https://github.com/actions/toolkit/pull/792): Cache: Delete temporary archive after upload** (Fixed [#791](https://github.com/actions/toolkit/issues/791))
  - The `saveCache` function in the actions toolkit used to leave temporary copies of each archive it uploaded.
    This could lead to the runner's SSD filling up, resulting in workflow run failure.
  - This PR fixed the issue by deleting the temporary files once they were no longer needed. 
* **[jenkinsci/build-time-blame-plugin#59](https://github.com/jenkinsci/build-time-blame-plugin/pull/59): Add tooltips and run links to graph** (Implemented [#57](https://github.com/jenkinsci/build-time-blame-plugin/issues/57))
  - This PR added imagemaps to the server-generated graph images, allowing users to click on the graph to jump to the relevant run.
  - It also shows timing details for each run in a tooltip on hover.
* **[moby/moby#41884](https://github.com/moby/moby/pull/41884): Fix spurious error from "docker load"** (Fixed [#41829](https://github.com/moby/moby/issues/41829))
  - (moby is the open-source upstream project for Docker)
  - Inconsistencies in Go's (un)marshalling of Time structs could create a mismatch between the histories of an image and its parent.
    This caused an error when loading images that were based on specific Windows images.
  - This PR worked around the issue by comparing the "created" times with `Equal`, avoiding the mismatch.
  - Confirmed fix in docker CLI v24 (appears to have landed in docker CLI v23).
* **[OWASP/NodeGoat#224](https://github.com/OWASP/NodeGoat/pull/224): Add GitHub Actions "E2E Test" workflow**
  - This PR ported the existing CI/CD workflow from Travis CI to GitHub Actions.
* **[OWASP/NodeGoat#227](https://github.com/OWASP/NodeGoat/pull/227): Add lint workflow**
  - This PR added a new GitHub Actions workflow that runs jshint over the codebase.
  - Previously linting needed to be run locally before submitting contributions.
* **[OWASP/NodeGoat#230](https://github.com/OWASP/NodeGoat/pull/230): Enable video recording and upload on failure**
  - This PR configured Cypress to record videos of each test, then made the workflow upload the videos and associated screenshots on failure.
* **[satackey/action-docker-layer-caching#82](https://github.com/satackey/action-docker-layer-caching/pull/82): Save only when new images were created** (Fixed [#81](https://github.com/satackey/action-docker-layer-caching/issues/81))
  - Previously the action treated restored images as "new", and reuploaded them as a new cache.
  - This PR fixed the issue by tracking restored and newly built images separately.
* **[satackey/action-docker-layer-caching#89](https://github.com/satackey/action-docker-layer-caching/pull/89): Use path module instead of string manipulation** (Fixed [#87](https://github.com/satackey/action-docker-layer-caching/issues/87))
  - The code that produces separate caches for each layer was being skipped on Windows due to path separator mismatch.
  - This PR rewrote the path handling to use the Node.js path module instead of string manipulation, avoiding the mismatch.

<details markdown="1"><summary>More...</summary>

* **[checkstyle/checkstyle#11014](https://github.com/checkstyle/checkstyle/pull/11014): infra: Set permissions required by the workflow**
* **[checkstyle/checkstyle#11016](https://github.com/checkstyle/checkstyle/pull/11016): infra: Fix branch name (remove quotes)**
* **[checkstyle/checkstyle#11024](https://github.com/checkstyle/checkstyle/pull/11024): infra: Update permissions for codeql workflow**
* **[containers/podman#10132](https://github.com/containers/podman/pull/10132): [CI:DOCS] Restore missing content to manpages**
* **[containers/podman#10167](https://github.com/containers/podman/pull/10167): [CI:DOCS] Improve titles of command HTML pages** (Fixed [#385](https://github.com/containers/podman.io_old/issues/385))
* **[containers/podman#10179](https://github.com/containers/podman/pull/10179): [CI:DOCS] Add titles to remote docs (windows)** (Fixed [#385](https://github.com/containers/podman.io_old/issues/385))
* **[containers/podman#10255](https://github.com/containers/podman/pull/10255): [CI:DOCS] Stop conversion of `--` into en dash** (Fixed [#389](https://github.com/containers/podman.io_old/issues/389))
* **[criteo/JVips#83](https://github.com/criteo/JVips/pull/83): Fix docker layer caching in build workflow**
* **[jenkinsci/build-time-blame-plugin#62](https://github.com/jenkinsci/build-time-blame-plugin/pull/62): Fix cast exception when a run has a missing step** (Fixed [#61](https://github.com/jenkinsci/build-time-blame-plugin/issues/61))
* **[OWASP/NodeGoat#214](https://github.com/OWASP/NodeGoat/pull/214): Fix intermittent test failures**
* **[OWASP/NodeGoat#218](https://github.com/OWASP/NodeGoat/pull/218): Fix repeated dbResets so tests run faster**
* **[OWASP/NodeGoat#223](https://github.com/OWASP/NodeGoat/pull/223): Fix Travis config warnings** (Fixed [#219](https://github.com/OWASP/NodeGoat/issues/219))
* **[satackey/action-docker-layer-caching#90](https://github.com/satackey/action-docker-layer-caching/pull/90): Detect updated "cache exists" error message** (Fixed [#85](https://github.com/satackey/action-docker-layer-caching/issues/85))
</details>

#### CI/CD Security

Fixes for various security issues in GitHub Actions/Workflows:

* **[hmarr/debug-action#5](https://github.com/hmarr/debug-action/pull/5): Block unexpected workflow commands**
* **[marocchino/sticky-pull-request-comment#332](https://github.com/marocchino/sticky-pull-request-comment/pull/332): Fix: Ignore other users' comments**
  - This action used a hidden flag string to mark the comments it created, allowing them to be found for update/replacement later.
    Any user could include that flag string in a PR comment, which would let them inject content into the action's comments.
  - This could be used for social engineering, as the message appears to be written by a trusted bot account.
  - This PR fixed the issue by confirming that the expected user posted the comment that was found.
* **[neovim/neovim#16034](https://github.com/neovim/neovim/pull/16034): ci(labeler): Remove checkout steps**
* **[nodejs/nodejs.dev#2115](https://github.com/nodejs/nodejs.dev/pull/2115): tools: Restrict workflow permissions**
* **[OWASP/OWASP-VWAD#147](https://github.com/OWASP/OWASP-VWAD/pull/147): Prevent code injection**
* **[OWASP/wstg#815](https://github.com/OWASP/wstg/pull/815): Prevent code injection**
* **[OWASP/www-community#518](https://github.com/OWASP/www-community/pull/518): Prevent code injection**

Most were code injections into bash/js scripts run as part of the workflow.


### Game Development

The EpicGames GitHub repositories are private, access instructions are at: [https://www.unrealengine.com/ue-on-github](https://www.unrealengine.com/ue-on-github)

* **[EpicGames/UnrealEngine#8141](https://github.com/EpicGames/UnrealEngine/pull/8141): Fix tooltip/watch text for by-ref function inputs**
  - This PR fixed incorrect display of "by reference" parameter values in the blueprint debugger.
* **[EpicGames/UnrealEngine#8306](https://github.com/EpicGames/UnrealEngine/pull/8306): Add DrawSpline to Widget Blueprint library**
* **[EpicGames/UnrealEngine#8406](https://github.com/EpicGames/UnrealEngine/pull/8406): [UE5] Slate line/spline rendering fixes**
  - This PR fixed various issues with slate line and spline rendering.
  - Fixing the issues required a rewrite of the line geometry setup code and pixel shaders (HLSL and GLSL).
  - The original code and this PR are based on ideas from [GPU Gems 2, Ch 22: Fast Prefiltered Lines](https://developer.nvidia.com/gpugems/gpugems2/part-iii-high-quality-rendering/chapter-22-fast-prefiltered-lines),
    extended to handle line strips as well as individual line segments.
  - This functionality is heavily used by the editor, for example, in timeline curves.
* **[EpicGames/UnrealEngine#10524](https://github.com/EpicGames/UnrealEngine/pull/10524): Slate: Fix aliased glyph quad edges** (PENDING)
  - This PR fixes a regression in Slate text rendering which causes aliasing on glyph quad boundaries
  - The issue can be seen on text with subpixel translation, or render transform rotation/scale/shear.
  - It's particularly noticeable in VR, where UI tends to be floating in world space.
    In that case the text is almost never aligned to the display grid, so the lack of antialiasing becomes a lot more apparent.
  - It also shows up in credits screens. The interior of each glyph scrolls smoothly, but glyph quad boundaries are effectively snapped to the nearest pixel.

Note that accepted changes are submitted to Epic's internal Perforce, which is upstream of the GitHub repo.
The PRs get closed rather than merged, but the commits include references to the original PR so you can see
which branch(es) they've landed in.

### Documentation

* **[actions/runner#1379](https://github.com/actions/runner/pull/1379): Update Required Dev Dependencies**
* **[actions/toolkit#905](https://github.com/actions/toolkit/pull/905): core: Update "core.error" documentation**
* **[docker/docs#11793](https://github.com/docker/docs/pull/11793): Explain Compose file relative path resolution**
* **[github/docs#9874](https://github.com/github/docs/pull/9874): Actions: Document "notice" workflow command**
* **[github/docs#10376](https://github.com/github/docs/pull/10376): Add missing "Open Git Bash" step**
* **[mdn/content#8915](https://github.com/mdn/content/pull/8915): Clarify CSS injection timing**
* **[nodejs/nodejs.dev#2118](https://github.com/nodejs/nodejs.dev/pull/2118): fix: Update links to match section header IDs**
* **[OWASP/NodeGoat#235](https://github.com/OWASP/NodeGoat/pull/235): Update links in A9 Tutorial description** (Fixed [#234](https://github.com/OWASP/NodeGoat/issues/234))
