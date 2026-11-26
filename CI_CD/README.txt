GROUND STATION CI/CD DOCUMENTATION
===================================

Location
--------
The GitHub Actions workflow definitions are stored in:
  .github/workflows/

This documentation is stored in:
  CI_CD/README.txt

1. .github/workflows/ci.yml — Ground station CI
------------------------------------------------
Runs automatically for pull requests targeting main, pushes to main, and
manual workflow dispatches.

What it does:
- Checks out the repository.
- Sets up Python 3.11.
- Detects Python source and project dependency manifests.
- Installs requirements.txt dependencies when present.
- Installs pytest and Ruff for automated test and lint checks.
- Compiles Python source files to catch syntax errors.
- Runs Ruff lint checks.
- Runs pytest when a tests directory or test files are present.
- Clearly reports that physical SDR/Raspberry Pi tests are not run on the
  standard GitHub-hosted runner.

Hardware testing:
Normal CI should mock SDR input and hardware interfaces. Physical hardware-in-
the-loop testing should use a dedicated, secured self-hosted Raspberry Pi
runner connected to the SDR. Do not expose that runner to untrusted pull
requests or arbitrary fork code.

2. .github/workflows/release.yml — Station release packaging
------------------------------------------------------------
Runs when a version tag beginning with "v" is pushed, or when manually started.

What it does:
- Checks out the selected source revision.
- Creates a versioned source archive from the Git commit.
- Generates a SHA-256 checksum for the archive.
- Uploads the archive and checksum as a GitHub Actions artifact.

This workflow does not install or update software on the Raspberry Pi. It
creates a traceable release package that can later be distributed using an
approved station update mechanism.

3. Deployment and hardware notes
--------------------------------
The station update mechanism has not yet been selected. No SSH deployment,
automatic updater, or remote Raspberry Pi installation is configured.
Raspberry Pi OS image details, Python runtime compatibility, SDR driver
installation, and hardware test procedures must be documented before enabling
automated station deployment.

Use a dedicated hardware runner only after it is secured and registered.
Keep production station credentials out of repository files and logs.
