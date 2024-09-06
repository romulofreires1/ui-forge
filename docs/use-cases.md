
# UIForge Use Cases

## Use Case 1: Dynamic Homepage
A product team wants to create a dynamic homepage for their mobile app that can be easily updated without requiring a new app release. By using UIForge, they can create a homepage template in the UIForge management interface, defining both local and remote data sources.

- **Template:** The homepage includes a welcome header and a list of products fetched from an external API.
- **API Response:** The UIForge API returns the homepage template and fetches the latest product data from the external API.
- **Renderer:** The mobile app's renderer interprets the template and displays the homepage with up-to-date product listings.

## Use Case 2: Personalized User Dashboard
The marketing team wants to show personalized content to users based on their profile data. UIForge allows them to create a user dashboard template that pulls data from a local source (e.g., user profile) and a remote source (e.g., recommended products).

- **Template:** The user dashboard includes a welcome message with the user's name and a list of recommended products.
- **API Response:** UIForge fetches the user profile from the local data source and retrieves recommended products from a remote API.
- **Renderer:** The renderer displays the personalized content based on the fetched data.

## Use Case 3: Rapid UI Changes
The business team wants to quickly change the UI layout for a seasonal promotion. They can log into the UIForge interface, create a new template for the promotion, and define new components and data sources.

- **Template:** The promotional layout includes banners, a countdown timer, and links to featured products.
- **API Response:** The template is retrieved from the UIForge database, and the remote data source fetches the featured products.
- **Renderer:** The app displays the new promotional layout immediately, without needing a new release.
