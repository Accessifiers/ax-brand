# Open edX Brand Package Interface

This project contains the Accessifiers' branding assets and styles used in Open edX applications. The file structure serves as an interface for implementing custom branding and theming in Open edX.

## Table of Contents

- [How to Use This Package](#how-to-use-this-package)
  - [Local Development (Hot Reload)](#local-development-hot-reload)
  - [Deployment](#deployment)
- [Additional Resources](#additional-resources)

## How to Use This Package

Applications in Open edX are configured by default to include this package for branding assets and visual style theming.

### Local Development (Hot Reload)

1. **Fork and Clone the MFE Repository:**

   Fork and clone the Micro Frontend (MFE) repository you want to customize. Refer to the [tutor-mfe README](https://github.com/overhangio/tutor-mfe#mfe-development) for detailed setup instructions.

   In your cloned MFE repository, create a file called `module.config.js` with the following content:

   ```javascript
   module.exports = {
     localModules: [{ moduleName: "@edx/brand", dir: "../ax-brand" }],
   };
   ```

   This configuration will look for the custom brand package in the same parent directory and apply the custom styling.

2. **Fork or Copy This Project:**

   Ensure that this project resides in the same parent directory as the MFE repository.

3. **Bind Mount the Custom Branding:**

   Run the following command to bind mount the custom branding onto the MFE application:

   ```bash
   tutor mounts add <MFE name>:<path-to-custom-brand>:/openedx/ax-brand
   ```

   - If working with multiple MFE pages, mount each individually using the same command.
   - Ensure that the names of the MFE pages match exactly.
   - For more details, refer to [How to Use edx-brand in Local Development and Production](https://discuss.openedx.org/t/how-to-use-edx-brand-in-local-development-and-production/13323).

4. **Replace Assets with Your Own SASS Theme:**

   Replace the assets in this project with your own SASS theme.

5. **Launch the Development Environment:**

   Run:

   ```bash
   tutor dev launch
   ```

   Any changes you make in your custom brand package will be hot-reloaded.

   > **Note:** Refresh the page if changes are not immediately reflected. It might take a few seconds for the SASS to be recompiled.

### Deployment

You can either [create a Tutor plugin](https://docs.tutor.edly.io/tutorials/plugin.html) or use [tutor-indigo](https://github.com/Accessifiers/ax-tutor-indigo)'s existing `plugin.py` to deploy your MFE changes.

1. **Add the Following Patch:**

   Add this patch to your custom plugin or `tutor-indigo`'s `plugin.py` to install your custom brand package:

   ```python
   hooks.Filters.ENV_PATCHES.add_item(
       (
           "mfe-dockerfile-post-npm-install",
           """
   RUN npm install '@edx/brand@git+<url-to-your-custom-brand-repository>'
   """
       )
   )
   ```

2. **Rebuild Your MFE Image:**

   Run:

   ```bash
   tutor images build --no-cache --no-registry mfe
   ```

3. **Launch or Start Tutor:**

   Run one of the following commands:

   ```bash
   tutor local launch
   ```

   or

   ```bash
   tutor local start -d
   ```

   > **Note:** Docker attempts to run as many build processes in parallel as possible, which can cause failures in the MFE image build. If you encounter OOM issues, RAM starvation, or network failures during NPM installs, try the following before restarting:

   ```bash
   cat >buildkitd.toml <<EOF
   [worker.oci]
     max-parallelism = 1
   EOF
   docker buildx create --use --name=singlecpu --config=./buildkitd.toml
   ```

   This forces Docker to build using a single CPU, reducing the likelihood of errors.

## Additional Resources

- [Official tutor-mfe README](https://github.com/overhangio/tutor-mfe)
- [Tutor MFE Custom Theme/Styling Help](https://discuss.openedx.org/t/tutor-mfe-custom-theme-styling-help/14170)
- [Video tutorial](https://youtu.be/9ED7PmzazIY)
