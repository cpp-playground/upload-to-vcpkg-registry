# upload-to-vcpkg-registry
Action used to add or update a C++ port to a given  Git vcpkg registry.

## Usage

### Requirements
This action has the following requirements:
- The `package_version` input must match an existing release tag.
- The `registry_access_token` must  be an access token giving read and write access to the registry repository
### Portfile variables substitution

In order to facilitate the portfile creation, some variable substitution are made:
- `COMMIT_REF` is substitued by the provided package version.
- `ARCHIVE_SHA` is substitued by the actual SHA512 of the archive generated for the provided version. 

To allow this Action to set the `REF` and `SHA512` fields automatically, the provided port file should look 
like this:

```cmake
vcpkg_from_github(
  OUT_SOURCE_PATH SOURCE_PATH
  REPO owner/repository
  REF COMMIT_REF # COMMIT_REF will be substitued by the provided package version
  SHA512 ARCHIVE_SHA  # ARCHIVE_SHA will be computed based on the package version
  HEAD_REF main
)
```

## Inputs

| **Input**                     | **Description**                                                                      |    **Default**    | **Required** |
| :---------------------------- | :----------------------------------------------------------------------------------- | :---------------: | :----------: |
| **`package_name`**            | Name of the package to upload to the vcpkg registry.                                 |                   |  **true**    |
| **`package_version`**         | Version of the package. Must match an already released tag.                          |                   |  **true**    |
| **`package_dep_file`**        | Path to the vcpkg.json file for the package relative to the source repository root.  |   `./vcpkg.json`  |  **false**   |
| **`package_port_file`**       | Path to portfile.cmake file for the package relative to the source repository root.  |`./portfile.cmake` |  **false**   |
| **`registry`**                | vcpkg target registry (owner/repository)                                             |                   |  **true**    |
| **`registry_access_token`**   | Token used to access the target registry                                             |                   |  **true**    |
