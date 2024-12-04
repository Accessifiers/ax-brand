# Open edX Brand Package Interface

This project contains the Accessifier's branding assets and style used in Open edX applications.

The file structure serves as an interface to be implemented for custom branding and theming of Open edX.

## How to use this package

Applications in Open edX are configured by default to include this
package for branding assets and theming visual style.

To use a custom brand and theme\...

### Local Development (Hot Reload)
1.  Fork and clone the MFE repository you want to customize. Refer to https://github.com/overhangio/tutor-mfe?tab=readme-ov-file#mfe-development for more detailed setup.

2.  Fork or copy this project. Ensure that it resides in the same directory as the MFE repository.

3.  Run `tutor mounts add <MFE name>:<path-to-cutom-brand>:/openedx/ax-brand` to bind mount the custom branding onto the MFE page. If you are doing multiple MFE pages, you will have to mount each of them individually using the same command. Make sure that the name of the MFE pages matches exactly. For example, refer to https://discuss.openedx.org/t/how-to-use-edx-brand-in-local-development-and-production/13323 for further setup details.

4.  Replace the assets in this project with your own SASS theme.

5.  Run `tutor dev launch`. (Refresh the page if the changes are not reflected immediately. It might take a few seconds for the SASS to be recompiled).

### 