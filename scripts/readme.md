These scripts handle deployment of SignBank through GitHub actions.

# Prelude: Deployment/Running Signbank
Upon build, Signbank produces a binary and a systemd service which runs it. To run Signbank on a server, you'll want that systemd service to be running so it can keep the binary up.

## Postgres Setup
Signbank requires the `citext` postgres extension to function. Additional postgres requirements are covered in the readme.md.

## Signbank.conf
Signbank.conf defines several environment variables, and lives at /etc/signbank.conf. It has the same puropse as the .env file, an example for which can be found at signbank/example.env. You can refer to that file for example values.

# Actions Process
Using GitHub actions requires dealing with some one-time credentials in the runner. These are done in the 'Create and set up SSH key' step of the YAML. The build artefact is then downloaded, and ./deploy.sh is made runnable. The build artifact is SCPd into the production server, then uses cat/ssh to run deploy.sh on the target server.

# deploy.sh
Untars a build artifact, then makes the necessary directories and moves our files into them. Restarts our systemd services so Signbank can update.
