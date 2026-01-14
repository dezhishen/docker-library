# docker-library

English | [简体中文](README-zh.md)

## Overview

This project provides the **base image** maintained by the OpenListTeam, designed to simplify and accelerate the process of building custom images for OpenList. It aims to reduce complexity and time consumption for all contributors and users.

## Building the Image Yourself

To build the image independently, follow these steps:

1. **Fork** this repository to your own GitHub account.
2. **Configure variables and secrets**:  
   Add the required variables and secrets in the Actions settings of your forked repository. See the [Variables and Secrets](#variables-and-secrets) section below for details.
3. **Trigger the build workflow**:  
   Navigate to the "Actions" tab in your repository and manually launch the relevant workflow.

## Variables and Secrets

| Name               | Type      | Description                                                                    | Required | Default Value            |
|--------------------|-----------|--------------------------------------------------------------------------------|----------|--------------------------|
| `IMAGE_NAME`           | variable  | The name of the Docker image to be built.                                      | No       | `openlist-base-image`    |
| `DOCKERHUB_ORG_NAME`   | variable  | The Docker Hub organization or user to which the image will be pushed.         | No       | Repository owner         |
| `GHCR_ORG_NAME`        | variable  | The GitHub Container Registry (GHCR) organization or user for image push.      | No       | Repository owner         |
| `DOCKERHUB_TOKEN`      | secret    | Docker Hub account token for authentication when pushing images.               | Yes      | N/A                      |

**Note:** If a variable is not specified, the repository owner name will be used as the default organization/user for Docker Hub and GHCR.

## Additional Notes

- Images built with the `aria2` and `aio` tags will be deprecated soon.  
  Please consider using dedicated images for these functionalities instead.

## Contributing

Contributions, issues, and feature requests are welcome!  

## License

This project is licensed under the [MIT License](LICENSE).

---