# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Appsmith application repository. Appsmith is a low-code platform for building internal tools and dashboards. The application is defined through JSON configuration files rather than traditional code.

## Repository Structure

```
ai-demo/
├── application.json       # Main application configuration
├── metadata.json         # File format and schema version metadata
├── theme.json           # UI theme settings
└── pages/               # Application pages
    └── Page1/
        ├── Page1.json   # Page layout and canvas configuration
        └── widgets/     # Widget definitions (forms, buttons, inputs, etc.)
```

## Architecture

### Application Model

- **application.json**: Defines the root application settings including:
  - `applicationVersion`: Version of the Appsmith application format (currently 2)
  - `evaluationVersion`: JavaScript evaluation engine version
  - `applicationDetail`: UI configuration (positioning, navigation, theme settings)
  - `pages[]`: List of pages in the application

### Widget System

Widgets are defined as individual JSON files in `pages/{PageName}/widgets/` directory. Each widget JSON contains:

- **Layout properties**: `topRow`, `bottomRow`, `leftColumn`, `rightColumn` for positioning
- **Styling**: Dynamic bindings to theme values like `{{appsmith.theme.colors.primaryColor}}`
- **Behavior**: Event handlers (e.g., `onClick`), validation rules, visibility conditions
- **Data**: Static or dynamic data bindings using JavaScript expressions in `{{}}`

Common widget types:
- `FORM_WIDGET`: Container for form elements
- `INPUT_WIDGET_V2`: Text input fields
- `SELECT_WIDGET`: Dropdown selectors
- `BUTTON_WIDGET`: Action buttons
- `CANVAS_WIDGET`: Layout containers

### Data Flow

- **Dynamic Bindings**: Use `{{expression}}` syntax for JavaScript expressions
- **Global Store**: Access via `appsmith.store` for application-wide state
- **Actions**: Defined in widget properties (e.g., `onClick: "{{storeValue('name', Input1.text)}}"`)
- **Widget References**: Reference other widgets by name (e.g., `Input1.text`)

## Development Workflow

### Editing the Application

This repository is typically synced with an Appsmith workspace via Git Sync. Changes can be made:

1. **Via Appsmith UI**: Edit at the application URL (see README.md), changes sync to Git
2. **Via JSON files**: Direct edits to widget/page JSON files, then pull into Appsmith

### Making Widget Changes

When modifying widgets:

1. Locate the widget JSON in `pages/{PageName}/widgets/{ParentWidget}/{WidgetName}.json`
2. Key properties to modify:
   - `leftColumn`, `rightColumn`, `topRow`, `bottomRow`: Widget positioning
   - `isVisible`, `isDisabled`: Widget state
   - `onClick`, `onChange`: Event handlers (use `{{JavaScript}}` syntax)
   - Widget-specific properties (e.g., `defaultText` for inputs, `sourceData` for selects)
3. Maintain proper JSON structure and preserve `widgetId` and `key` fields
4. Use dynamic bindings with `{{}}` for computed values

### Adding New Widgets

1. Create new JSON file in appropriate `pages/{PageName}/widgets/` directory
2. Generate unique `widgetId` and `key` values
3. Set proper parent-child relationships via `parentId`
4. Register in parent widget's `children` array if applicable
5. Define position within parent's coordinate system

## Important Notes

- **Git Sync**: This repository is connected to an Appsmith workspace. Direct commits may be overwritten if not coordinated with UI changes
- **Widget IDs**: The `widgetId` field must be unique across the application
- **Coordinate System**: Widgets use a grid-based positioning system with rows and columns
- **Theme Bindings**: Use `{{appsmith.theme.*}}` for consistent theming
- **Mobile Layouts**: Separate mobile properties (e.g., `mobileBottomRow`) define responsive behavior
