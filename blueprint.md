# Project Blueprint

## Overview

This is a Flutter application scaffolded for web and mobile targets. The initial application is a simple counter, but the development environment is being configured to support additional capabilities, including physics simulation.

## Project Documentation

This section outlines the style, design, and features of the application as it is developed.

### Version 1.0: Initial Setup & Counter App

*   **Framework:** Flutter
*   **Platform Targets:** Web, Android, iOS, Linux
*   **User Interface:**
    *   Standard Material Design theme.
    *   A main screen with a centered counter.
    *   A floating action button (`+` icon) that increments the counter.
*   **Functionality:** The app counts the number of times the button is pressed.
*   **Testing:** A widget test (`test/widget_test.dart`) exists to verify the counter increment functionality.
*   **Deployment:** Deployment instructions are located in `docs/deployment.md`, covering Firebase Hosting.

### Current State: Environment Configuration

*   **Development Environment (`.idx/dev.nix`):**
    *   **Packages:**
        *   `pkgs.flutter`
        *   `pkgs.jdk21`
        *   `pkgs.unzip`
        *   `pkgs.mujoco` (added for physics simulation capabilities)
    *   **IDE Extensions:**
        *   `Dart-Code.flutter`
        *   `Dart-Code.dart-code`
    *   **Previews:** A web preview server is configured to run the application on `http://localhost:3000`.

## Current Task: Add MuJoCo to Environment

*   **Request:** "please ass pkgs.mujoco to dev.nix fole"
*   **Status:** Completed.
*   **Steps Taken:**
    1.  Modified the `.idx/dev.nix` file.
    2.  Added `pkgs.mujoco` to the list of environment packages.
    3.  Corrected a structural issue in the `idx.previews` configuration to resolve an error.
    4.  This blueprint has been created to track project progress.
