# Faerûn Wanderer

## Table of Contents
- [Introduction](#introduction)
- [Key Features](#key-features)
- [Architecture Overview](#architecture-overview)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Setup](#setup)
  - [Configuration](#configuration)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

## Introduction

Faerûn Wanderer transforms your real-world activity into a Dungeons & Dragons-inspired journey through the magical world of Faerûn. This application gamifies your daily walks and fitness routine by integrating your steps into a rich fantasy role-playing experience.

## Key Features

*   ✅ **Gamified Fitness:** Track your real-world steps to level up your character and unlock new abilities.
*   ✅ **Dynamic World:** Discover monsters, find treasure, and visit iconic locations on a real-world map.
*   ✅ **Deep Progression:** Customize your character, equip powerful gear, and earn unique achievements.
*   ✅ **Immersive Storytelling:** Every step contributes to your adventurer's epic saga.

## Architecture Overview

The Faerûn Wanderer app transforms real-world steps into a D&D adventure using a robust cloud-based architecture, as depicted in the diagram above:

*   **Client (React/JavaScript):** Your device; the user interface that players interact with.
*   **Base44:** The primary platform connecting your client to the backend. It handles web hosting, user authentication, and securely routes all game logic requests to specialized backend functions.
*   **Azure Functions (JavaScript/Deno Proxies):** These are the Deno-based backend functions within this repository. They act as secure proxies, forwarding requests from the Base44 frontend to your separately deployed Azure Functions App.
*   **Azure Functions (External):** This refers to your separate Azure Functions App deployment (from the other ZIP file you mentioned). These functions contain the core game logic that interacts directly with Azure cloud services.
*   **Azure SQL Database:** The central database where all game data is stored, including player profiles, steps, inventory, monsters, locations, and achievement progress. Managed by the external Azure Functions.
*   **Azure Blob Storage:** Used for storing large, static assets like monster images, served efficiently by the external Azure Functions.
*   **Azure Maps:** Provides the location-based map functionality and geospatial services, enabling real-world GPS tracking and in-game element placement. Accessed via the external Azure Functions and directly by the client.

In essence, your app (Client) communicates with Base44, which then orchestrates proxy calls to your external Azure Functions. These external functions then manage data in Azure SQL, assets in Azure Blob Storage, and interact with Azure Maps for a dynamic, location-aware gaming experience.

## Getting Started

Follow these instructions to set up and run the Faerûn Wanderer application. This setup assumes you have already deployed your external Azure Functions code to an Azure Functions App.

### Prerequisites

Before you begin, ensure you have the following installed:

*   **Node.js (LTS recommended) & npm/yarn:** For the Base44 frontend application.
*   **Deno:** For running the Base44 backend functions (which act as proxies to your external Azure Functions).
*   **An active Azure subscription:**
    *   You must have an **Azure Functions App** instance already deployed with the separate Azure Functions code (the second ZIP file you mentioned). This app serves as the core backend.
    *   Ensure your deployed Azure Functions App is correctly configured to connect to an **Azure SQL Database** and has access to **Azure Blob Storage**.
    *   You will also need an **Azure Maps** account and its **Subscription Key** for map functionality.

### Setup

1.  **Clone or Unzip this Repository:**
    Extract the contents of the "code of the app" ZIP file (or clone this repository) to your desired project directory. This repository contains the Base44 frontend and its Deno-based backend functions.

2.  **Install Frontend Dependencies:**
    Navigate to the root directory of the unzipped project (where `package.json` is located) and install the necessary dependencies:
    ```bash
    npm install
    # or
    yarn install
    ```

### Configuration

The Base44 backend functions (located in the `functions/` directory of this project) act as proxies to your *external* Azure Functions App. They require specific environment variables to know where to send requests and how to authenticate.

You will need to configure the following environment variables within your Base44 project's settings (typically via the Base44 dashboard under "Environment Variables"):

*   `AZURE_FUNCTION_APP_URL`: The base URL of your deployed Azure Functions App (e.g., `https://your-functions-app.azurewebsites.net`).
*   `AZURE_FUNCTION_API_KEY`: The API key for your Azure Functions App. This is crucial for authenticating requests from Base44 to your Azure Functions.
*   `AZURE_MAPS_SUBSCRIPTION_KEY`: Your subscription key for Azure Maps. This is used by the application to display maps and access geospatial services.

    *Note:* Ensure these environment variables are correctly set within your Base44 project for the backend functions to operate.

### Running the Application

Once your environment variables are configured in Base44, you can run the application. Typically, this is done through the Base44 development environment. The Base44 platform handles serving the frontend and automatically connecting it to your configured backend functions (which then proxy to your deployed Azure Functions).

## Project Structure

This repository (the "code of the app" part) is structured as follows:

. ├── components/ # Reusable React components for UI elements. ├── entities/ # JSON Schema definitions for client-side data models. ├── functions/ # Deno-based backend functions that proxy requests to Azure. ├── pages/ # React components defining the main application views/pages. ├── public/ # Static assets for the web application. ├── utils/ # General utility JavaScript functions. ├── Layout.js # The global layout component wrapping all pages. ├── package.json # Node.js project metadata and dependencies. └── ... # Other configuration files specific to the Base44 project.
