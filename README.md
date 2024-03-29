## Getting Started

### Prerequisites

- Node.js 10.0+
- A running instance of Saleor.

  To run the storefront, you have to set the `API_URI` environment variable to point to the Saleor GraphQL API. If you are running Saleor locally with the default settings, set `API_URI` to: `http://localhost:8000/graphql/`.

### Installing

Clone the repository:

```
git clone https://github.com/mirumee/saleor-storefront.git
```

Enter the project directory:

```
cd saleor-storefront
```

#### Dev

Install NPM dependencies:

```
npm i
```

Run the development server:

```
npm start
```

Go to `http://localhost:3001` to access the storefront.


## Creating new components

All new components should follow Atomic Design Guidelines and be placed in `src/@next/components` directory.

Files structure can be generated using `plop`:

```
npm run generate
```

## Modifying the Storefront

[From Spectrum Post](https://spectrum.chat/saleor/saleor-storefront/modifying-the-storefront~c1955dbf-a421-4fb6-b99e-937dd2642b23)

### Important Files

- **saleor-storefront/config/webpack/config.base.js** - Base webpack config file.
  - Can change name of the app (displayed when installed on mobile)
- **saleor-storefront/src/index.html** - Main template file that contains the <div id="root"></div>
  - Can change title of storefront here
- **saleor-storefront/src/index.tsx** - Main entry point file. Render's the <App /> component, apollo-client, and others to the root div in index.html file above.
- **saleor-storefront/src/core/config.ts** - Controls number of products shown per page, support email, gateway providers, social media, and some meta.
  - Can change support email
  - Can change products shown per page
  - Can change gateway providers
  - Can change social media links that are displayed in the footer
  - Can change some meta options
- **saleor-storefront/src/images/** - Holds all the images for logo, cart, favicon, etc.
  - Can change storefront logo, favicon, or add new images here.
- **saleor-storefront/src/globalStyles/scss/variables.scss** - Contains base styles like colors, font size, container width, media breakpoints and more.
- **saleor-storefront/src/@next/globalStyles/** - Contains more base styles, themes, media, and constants.
- **saleor-storefront/src/views/** - This folder controls the views, or what is displayed for each page. Most views have a file named "Page.tsx" that controls the layout of the page and a file named "View.tsx" that calls the query and renders the <Page /> component with the data.
  - Can add another view to storefront here. Requires adding a route (see routes below).
- saleor-storefront/src/@next/pages/ - Second spot for modifying/adding different pages. This is the recommended directory to add new pages.
- **saleor-storefront/src/app/routes/** - This folder contains the paths as well as holds the <Routes /> component. Here is where you add a new path and route.
  1.  Export a new path in paths.ts
  2.  Inside AppRoutes.tsx import your new view (see views above) and create a new route with path={paths.newPath} and component={newViewPage}
  3.  To link to your new view import { Link } from "react-router-dom" and use new path you created in paths.ts (make sure to import it)
- **saleor-storefront/src/app/App.tsx** - This is main <App /> component that renders the <MainMenu />, <Routes /> (explained below), <Footer /> and a couple other components.

### Adding a Payment Gateway

- **saleor-storefront/src/core/config.ts** - Add new gateway provider name here.
- **saleor-storefront/src/@next/components/organisms/** - Create a new folder for new payment gateway component here.
- **saleor-storefront/src/@next/components/organisms/PaymentGatewaysList/PaymentGatewaysList.tsx** - Import new gateway component, create a new switch case to handle your gateway component.

### Receiving confirmation emails

- **Set [EMAIL_URL](https://docs.saleor.io/docs/developer/running-saleor/configuration#setting-environment-variables) environment variable for Saleor core.**
  - Using Docker - Add EMAIL_URL as new enviornment variable to both the api and worker service following the format [here](https://docs.saleor.io/docs/developer/running-saleor/configuration#email_url).
- **Issues getting emails working?**
  - Gmail
    - Check to see that "Less secure app access" is turned ON. Under "Manage your Google Account" > Go to the security tab. By default, the setting is off for security reasons.
    - If using 2FA make sure to set an [app password](https://support.google.com/accounts/answer/185833?p=InvalidSecondFactor&visit_id=637355441414497566-1310044707&rd=1) and use that in place of your normal login password.
