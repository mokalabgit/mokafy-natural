# 📚 Development Guidelines

This repository is part of a modular architecture for Shopify theme development.  
Below is the defined workflow:

## Repository and Branch Structure

- **`base`** → Main repository, forked from Shopify’s official **Dawn** theme.
  - **`base.base`** → Branch where all **common components and configurations** for child themes are stored.
- **`natural`** → Child repository, forked from **`base.base`**.
  - **`natural.base`** → Main branch, always synced with `base.base`.
  - **`natural.natural`** → Branch for theme-specific customizations.

## Summary of Repository Structure

This is a high-level explanation of how our repositories are organized:

1. **Base**  
   This repository contains all our core development and is synced with Shopify's Dawn theme.  
   Its `main` branch is a clone of the `main` branch of the official Dawn repository.  
   The working branch for us is `base`.

2. **Themes**  
   These repositories contain the developments for specific theme products.  
   We can have multiple theme repositories. In this case, `natural` is the first, but more can be created.  
   Each theme repository has:
   - A `base` branch cloned from our `base` repository, which allows synchronization (`base > natural` or `base > [theme]`).
   - A main branch named after the theme itself (`natural`, `arabica`, etc.).

3. **Projects**  
   When starting a project, we decide which theme to use and create the project repository from the theme repository and its main branch.  
   For example:
   - If the project `acme` uses the `natural` theme:
     - The `acme` repository will have a `natural` branch to allow synchronization (`natural > acme`).
     - The main branch of the project will be named `acme`.
   - Another example:
     - If the project `zenith` uses the `arabica` theme:
       - The `zenith` repository will have a `arabica` branch.
       - The main branch of the project will be `zenith`.

This model allows clean separation between product development (`base`, themes) and customer project customizations.

## Core Principles

1. The **base** repository is used exclusively to maintain reusable components and shared configurations.
2. Theme-specific development is carried out within each child theme repository.
3. **Do not connect the `base` branch directly to Shopify.**  
   Shopify may automatically modify the code when using the page builder, and these changes should not be merged into `base.base` unless thoroughly reviewed.

## Recommended Workflow

1. Work from an alternative branch (`dev`) that mirrors `base`.
2. Create **feature branches** for every new development (`feature/your-new-feature`).
3. Connect alternative branches (`dev`, `feature/*`) to Shopify for real-time development and testing.
4. Use **cherry-pick** to selectively merge changes from secondary branches into `natural.base`.
5. Create **pull requests** from `natural.base` to `base.base` to share common improvements with the parent repository.

## Tailwind CSS & Development Environment

All new components should preferably be built using **Tailwind CSS** to ensure consistency and maintainability.  
We use [Tailwind CSS v4](https://tailwindcss.com/docs/installation) with native integration into the theme.

### Environment setup

1. Clone the repository.
2. Run `npm install` to install dependencies.
3. Use `npm run build:css` to generate the CSS once.
4. Use `npm run watch:css` during development to auto-compile on file changes.

The Tailwind setup is intentionally simple and designed to integrate smoothly with Shopify themes.

## Project Repositories

In addition to the `base` repository and the child themes (`natural`, etc.), an additional layer is defined for real-world projects that use these themes.  
The objective is to allow each project to maintain its own version control and customizations without interfering with the development and maintenance of the child theme.

### Structure and workflow

1. A **real project** (e.g., `acme`) starts as a **fork of the chosen child theme** (e.g., `natural`).
2. The project repository (`acme`) includes:
   - `acme.natural`: synchronized copy of `natural.natural`. This branch should **never be modified** manually.
   - `acme.acme`: branch for customer-specific customizations.
3. The `acme.acme` branch is the only branch used to apply customer-specific customizations:
   - Colors, logos, images
   - Component customization
   - Shopify-managed content adjustments
4. Periodically, `acme.natural` should be updated from upstream to receive improvements from the child theme.
5. Projects are **not expected** to create pull requests back to the child theme repository.  
   Only if a project team detects a valuable improvement that could be reused across all themes, a pull request can be submitted under the following conditions:
   - Client-specific customizations must **never** be pushed upstream.
   - Only general improvements or reusable fixes should be proposed.
6. It is recommended to tag stable versions of the project (`acme.acme`) before making significant changes to ensure traceability and easier future integrations.

### Theme and Project Hierarchy

| Repository Type | Repository | Branch | Purpose |
|-----------------|------------|--------|---------|
| Base repository | base       | base.base | Shared components and configurations |
| Child theme repository | natural    | natural.base | Synced with `base.base` |
|                 |            | natural.natural | Theme-specific customizations |
| Project repository | acme     | acme.natural | Synced with `natural.natural` (do not modify) |
|                 |            | acme.acme | Customer-specific customizations |

Each level inherits from the previous one but **maintains full independence for local customizations**.

### Additional notes

- This system allows the `natural` theme to evolve as a product with shared improvements without being polluted by customer-specific changes.
- Real project repositories (`acme`, etc.) are free to evolve independently without any obligation to contribute back.
- Each project team must maintain clear internal documentation of any changes made outside the official flow to avoid issues when updating.
