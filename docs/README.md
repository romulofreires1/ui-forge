# UIForge Proposal

## Overview
UIForge is a server-driven UI ecosystem designed to dynamically generate interfaces based on templates provided by the server. It allows interfaces to be updated without requiring client-side application updates, ensuring flexibility and agility in deployments.

### Main Components
1. **Renderer (Client):**
   - The renderer is responsible for interpreting the templates provided by the API and rendering the user interface dynamically.
   - The management of local and remote components is handled by the renderer's technology, and users must implement their own component catalog.

2. **API of Templates:**
   - The API provides templates to the renderer and fetches data from both local and remote sources, as defined in the templates.
   - Each component may have its own `dataSource` (local or remote), and the data is returned in a `data` field, which is accessible to all components in the hierarchy.

3. **Local and Remote Data Sources:**
   - Components can define their own data sources, which can either be fetched from the server (local) or from an external API (remote).
   - Data from the `dataSource` is aggregated in the `data` field of the template and is visible to all components below the hierarchy.

4. **Template Management Interface:**
   - An intuitive interface allows product and business users to create and manage templates.
   - The interface includes a visual editor, version control, and a preview tool for testing templates.

## Templates

### General Template:
This template is used as a base structure for dynamic UI rendering. It defines multiple components and how data from local and remote sources is used to render the UI.

```json
{
  "templateId": "generalTemplate",
  "version": "1.0",
  "platform": "web",
  "components": [
    {
      "type": "Header",
      "props": {
        "title": "{context.datasources.headerSource.headerTitle}",
        "style": {
          "backgroundColor": "#4CAF50",
          "color": "#FFFFFF",
          "padding": "10px",
          "textAlign": "center"
        }
      },
      "dataSource": {
        "id": "headerSource",
        "type": "local",
        "value": {
          "headerTitle": "Welcome"
        }
      }
    },
    {
      "type": "Footer",
      "props": {
        "text": "Thank you for using our service!",
        "style": {
          "backgroundColor": "#4CAF50",
          "color": "#FFFFFF",
          "padding": "10px",
          "textAlign": "center"
        }
      },
      "dataSource": {
        "id": "footerSource",
        "type": "local",
        "value": {
          "footerText": "Thank you for visiting our store!"
        }
      }
    }
  ]
}
```

### ExperimentRenderer Template:
The **ExperimentRenderer** component is specifically designed for A/B testing scenarios. It allows you to define multiple variants and a default variant, and renders one based on predefined logic.

```json
{
  "templateId": "experimentRendererTemplate",
  "version": "1.0",
  "platform": "web",
  "components": [
    {
      "type": "ExperimentRenderer",
      "props": {
        "variants": {
          "default": {
            "type": "Text",
            "props": {
              "text": "This is the default variant",
              "style": {
                "fontSize": "18px",
                "color": "#333",
                "marginBottom": "10px"
              }
            }
          },
          "variantA": {
            "type": "Text",
            "props": {
              "text": "This is Variant A",
              "style": {
                "fontSize": "18px",
                "color": "#FF5733",
                "marginBottom": "10px"
              }
            }
          },
          "variantB": {
            "type": "Text",
            "props": {
              "text": "This is Variant B",
              "style": {
                "fontSize": "18px",
                "color": "#33FF57",
                "marginBottom": "10px"
              }
            }
          }
        }
      }
    }
  ]
}
```

### DynamicRenderer Template:
The **DynamicRenderer** component is a more general-purpose version of the **ExperimentRenderer**. While the **ExperimentRenderer** is focused on A/B testing, the **DynamicRenderer** allows you to render different UI components dynamically based on a `type` value from a remote or local data source. Additionally, a default option is provided in case no value matches.

```json
{
  "templateId": "dynamicRendererTemplate",
  "version": "1.0",
  "platform": "web",
  "components": [
    {
      "type": "DynamicRenderer",
      "props": {
        "fieldToCheck": "{context.datasources.typeSource.fieldToCheck}",
        "types": {
          "SP": {
            "type": "Text",
            "props": {
              "text": "You selected São Paulo",
              "style": {
                "fontSize": "18px",
                "color": "#333"
              }
            }
          },
          "RJ": {
            "type": "Text",
            "props": {
              "text": "You selected Rio de Janeiro",
              "style": {
                "fontSize": "18px",
                "color": "#FF5733"
              }
            }
          },
          "MG": {
            "type": "Text",
            "props": {
              "text": "You selected Minas Gerais",
              "style": {
                "fontSize": "18px",
                "color": "#33FF57"
              }
            }
          },
          "default": {
            "type": "Text",
            "props": {
              "text": "No matching region selected",
              "style": {
                "fontSize": "18px",
                "color": "#999"
              }
            }
          }
        }
      },
      "dataSources": [
        {
          "id": "ufListSource",
          "type": "remote",
          "url": "https://api.example.com/ufs",
          "contextMapping": {
            "headers": "{context.request.headers}",
            "queryParams": "{context.request.queryParams}",
            "body": "{context.request.body}"
          }
        },
        {
          "id": "typeSource",
          "type": "local",
          "value": {
            "fieldToCheck": "uf"
          }
        }
      ]
    }
  ]
}
```

### Fallback Example with Monitoring
In this example, a `Button` is placed inside a `Container`, and if the button fails to render, an `EmptyComponent` is rendered as the fallback. The fallback also supports defining monitoring messages, which can be used for tracking or logging purposes.

```json
{
  "templateId": "containerWithButton",
  "version": "1.0",
  "platform": "web",
  "components": [
    {
      "type": "Container",
      "props": {
        "style": {
          "padding": "20px",
          "border": "1px solid #ddd",
          "display": "flex",
          "flexDirection": "column",
          "alignItems": "center"
        }
      },
      "children": [
        {
          "type": "Button",
          "props": {
            "text": "Click Me",
            "onClick": "handleButtonClick"
          },
          "fallback": {
            "type": "EmptyComponent",
            "props": {},
            "monitoring": {
              "action": "track",
              "level": "error",
              "message": "Button component failed to render."
            }
          }
        }
      ]
    }
  ]
}
```

## Use Cases

### Use Case 1: Dynamic Homepage
A product team wants to create a dynamic homepage for their mobile app that can be easily updated without requiring a new app release. By using UIForge, they can create a homepage template in the UIForge management interface, defining both local and remote data sources.

- **Template:** The homepage includes a welcome header and a list of products fetched from an external API.
- **API Response:** The UIForge API returns the homepage template and fetches the latest product data from the external API.
- **Renderer:** The mobile app's renderer interprets the template and displays the homepage with up-to-date product listings.

### Use Case 2: Personalized User Dashboard
The marketing team wants to show personalized content to users based on their profile data. UIForge allows them to create a user dashboard template that pulls data from a local source (e.g., user profile) and a remote source (e.g., recommended products).

- **Template:** The user dashboard includes a welcome message with the user's name and a list of recommended products.
- **API Response:** UIForge fetches the user profile from the local data source and retrieves recommended products from a remote API.
- **Renderer:** The renderer displays the personalized content based on the fetched data.

### Use Case 3: Rapid UI Changes
The business team wants to quickly change the UI layout for a seasonal promotion. They can log into the UIForge interface, create a new template for the promotion, and define new components and data sources.

- **Template:** The promotional layout includes banners, a countdown timer, and links to featured products.
- **API Response:** The template is retrieved from the UIForge database, and the remote data source fetches the featured products.
- **Renderer:** The app displays the new promotional layout immediately, without needing a new release.
