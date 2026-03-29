# GEMINI.md - Showcase Project

This document provides context and instructions for the **Gemini Showcase** project, a Spring Boot application designed to demonstrate integration and functionality.

## Project Overview

- **Name:** Gemini Showcase
- **Technology Stack:**
  - **Framework:** Spring Boot 4.0.5
  - **Language:** Java 17
  - **Build Tool:** Gradle (Kotlin DSL)
  - **Module:** Spring Web MVC (`spring-boot-starter-webmvc`)
- **Package Structure:** `live.study.gemini.showcase`
- **Architecture:** Standard Spring Boot web application layout.

## Building and Running

The project uses the Gradle wrapper (`gradlew`) for all build and management tasks.

### Core Commands
- **Run the application:**
  ```bash
  ./gradlew bootRun
  ```
- **Build the project:**
  ```bash
  ./gradlew build
  ```
- **Run tests:**
  ```bash
  ./gradlew test
  ```
- **Clean build artifacts:**
  ```bash
  ./gradlew clean
  ```

## Development Conventions

- **Style:** Adheres to standard Spring and Java naming conventions (PascalCase for classes, camelCase for variables/methods).
- **Testing:** Uses JUnit 5 for testing. Standard Spring Boot integration tests are located in `src/test/java`.
- **Configuration:** Main configuration is located in `src/main/resources/application.properties`.

## Directory Structure Highlights

- `src/main/java/live/study/gemini/showcase/`: Root package for the application source code.
- `src/main/resources/`: Contains application properties, static assets, and templates.
- `src/test/java/`: Contains unit and integration tests.
- `build.gradle.kts`: Project dependency and build configuration.
- `HELP.md`: Reference documentation for Spring Boot and Gradle plugins.
