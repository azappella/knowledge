# folder structure for node projects

```
project-root/
  💡 Documentations & diagrams on local development steps, deployment processes, etc
  /docs

  💡 If you prefer keeping testing files seperately
  /__tests__ 

  💡 Keep all your code files seperate from configuration files
  /src

    💡 REST API routes, keep them clean & short
    /routes

    💡 Responsible for receiving & returning data to routes
    /controllers

    💡 Core business logic
    /services

    💡 Database logic only (data-in/data-out, no business logic)
    /repositories

    💡 Optional, a place to define your DB schema if needed
    /models

    💡 Optional: Static values you might use across the project
    /constants

    💡 Wrappers for 3rd party SDKs/APIs, such as Stripe/Shopify APIs
    /libs

    💡 Parsing errors, protecting endpoints, caching, etc
    /middlewares

    💡 Optional: Type definitions if needed
    /types
    
    💡 Optional: Functions / classes for validating incoming API payloads
    /validators

    💡 Optional, only if you rely on generated types/functions
    /generated
    
    💡 Optional: Some teams call it 'utils' folder
    /common
    
    💡 Configure your logger here
    logger.ts

    💡 Centralise your environment variables in one place
    env.ts
  
  💡 Keep configuration files in root folder
  package.json
  .prettierrc
  .prettierignore
  .eslintrc.js
  .eslintignore
  yarn.lock
```