# Home Page

Home Page for Vince

## Development Environment

![](/docs/dev-containers-arch.png)

Prerequisites:
- `ms-vscode-remote.remote-containers` to run the site inside of a container.
- Docker Desktop to run the container.

The `.devcontainer` folder contains the necessary configuration files.

Once the configuration files are correct, run the project using the **Reopen in Container** command from the command palette.

### Tips

Port forwarding seems to happen automatically once the Jekyll web server is running.

Do **not** try and put all the Jekyll-related stuff in a sub-folder of the repo (e.g. `website`) because the markdown image links in the vscode markdown previewer become incompatible with ones that work out the output files in `_site`.

## Local Development Site

Only run this command once, to create a brand new website.
```sh
# creates a boilerplate site in the /website directory
# notably the Gemfile is created which itself contains good documentation
jekyll new website
```

When creating new containers, run this command each time. It cannot (easily) be part of the Dockerfile because the dev containers extension hasn't yet created the bind mount to the code repo, so the Gemfile doesn't exist in the container yet.
```sh
bundle install
# this warning can be safely ignored, because no other users are in the container
# Don't run Bundler as root. Installing your bundle as root will break this application for all non-root users on this machine.
```

Run this command to start the server. Do ctrl-c then rerun to see changes.
```sh
bundle exec jekyll serve
```

## Deployment
Push changes to the main branch of this git repository.

Open the [onebox](https://github.com/VincentSaelzler/onebox]) repo in a separate dev container.

Pull changes, and build the site.
```sh
cd ansible/
# ans 7.0-ct-our-journey.yml
# this will be different
```