# Example

This is an example project to repackage [appimage] to [linglong] package.

[appimage]: https://docs.appimage.org/
[linglong]: https://linglong.dev/

Check Makefile and linglong.yaml for details.

- [play_2048](./io.github.mgrojo.Play2048)

  <https://github.com/mgrojo/play_2048>

# Project struct

Each folder contains
the packaging scripts and configuration files for one application.

Each folder should be named as the application ID.

Application ID should be a valid [XDG desktopfile ID], which means that:

> It should start with a reversed DNS domain name
> controlled by the author of the application, in lower case.
>
> The domain name should be followed by the name of the application,
> which is conventionally written with
> words run together and initial capital letters (CamelCase).
>
> For example,
> if the owner of example.org writes "Foo Viewer",
> they might choose the name org.example.FooViewer.

[XDG desktopfile ID]: https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s02.html#desktop-file-id

If that application doesn't have any domain controlled,
then the recommend application ID format is `io.github.${username}.${appname}`,
as `${username}.github.io` is the domain github offer to user.

Each folder should contain:

- a `.gitignore` to make sure
  that this git repository is clean when the packaging is done.
- a `Makefile` to extract appimage and install it.
- a `linglong.yaml` to
  let linglong-builder repackage that appimage file into a linglong package.

# Build

`ll-builder build`

# Run

`ll-builder run`
