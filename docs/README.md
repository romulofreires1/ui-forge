
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

3. **PostgreSQL Database:**
   - The database stores the templates and metadata necessary for the system.
   - The templates are versioned and stored in JSON format.

4. **Local and Remote Data Sources:**
   - Components can define their own data sources, which can either be fetched from the server (local) or from an external API (remote).
   - Data from the `dataSource` is aggregated in the `data` field of the template and is visible to all components below the hierarchy.

5. **Template Management Interface:**
   - An intuitive interface allows product and business users to create and manage templates.
   - The interface includes a visual editor, version control, and a preview tool for testing templates.

## Data Flow
1. The client requests a specific template from the API.
2. The API retrieves the template from the database.
3. The API fetches data from both local and remote sources as defined in the template.
4. The template and its corresponding data are returned to the client.
5. The renderer interprets the template and renders the UI.

### Template Example:
```json
{
  "templateId": "homepage_v1",
  "components": [
    {
      "type": "header",
      "title": "Welcome",
      "dataSource": { "type": "local", "path": "/local/user" }
    },
    {
      "type": "list",
      "dataSource": { "type": "remote", "url": "https://api.example.com/products" }
    }
  ],
  "data": {
    "user": { "name": "John Doe" },
    "products": [...]
  }
}
```

## PostgreSQL Database Schema

### Template Table Schema:
```sql
CREATE TABLE templates (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    version INTEGER,
    template JSONB,
    created_at TIMESTAMP DEFAULT NOW()
);
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

## Project Structure

```bash
UIForge/
│
├── api/                  # Código relacionado à API
│   ├── src/              # Código-fonte da API
│   └── README.md         # Instruções da API
│
├── db/                   # Scripts do banco de dados
│   ├── migration.sql      # Script de criação das tabelas
│   └── README.md         # Informações do banco de dados
│
├── docs/                 # Documentação do projeto
│   ├── architecture.md   # Explicação da arquitetura
│   └── use-cases.md      # Exemplos de casos de uso
│
├── templates/            # Exemplos de templates (JSON)
│   └── homepage.json
│
└── README.md             # Visão geral do projeto
```

## Conclusion
UIForge provides a flexible, dynamic, and scalable way to manage UI templates from the server side, allowing business teams to make rapid changes to the UI without the need for client-side updates. Its integration with PostgreSQL for storing templates and the ability to fetch data from both local and remote sources provides a powerful tool for dynamic UIs.