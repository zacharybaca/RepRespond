# RepRespond Web Application \- Project Checklist

This checklist is designed to guide a junior developer through building the Template Recommendation Tool from scratch using the MERN stack.

# **Phase 1: Project Setup**

- [ ] Initialize Git repository for the project.

      * Run \`git init\` in the root folder of the project.

      * Create a \`.gitignore\` file and include \`node\_modules\`, \`.env\`, and \`dist\` to avoid committing build files or secrets.

      * Make an initial commit using \`git add .\` and \`git commit \-m "Initial project commit"\`.

- [ ] Setup root directory with "client" and "server" folders.

      * Create a project root directory named \`template-recommendation-tool\` and navigate into it.

      * Create a \`server\` directory for Express/MongoDB backend logic.

      * Create a \`client\` directory (or allow Vite to generate it) for React frontend logic.

- [ ] Server: Run \`npm init \-y\` and install dependencies (express, mongoose, dotenv, cors, jsonwebtoken, bcryptjs).

      * Navigate to the \`server\` directory: \`cd server\`.

      * Initialize Node project: \`npm init \-y\` to generate \`package.json\`.

      * Install core production packages: \`npm install express mongoose dotenv cors jsonwebtoken bcryptjs\`.

      * Install development dependencies for continuous reloading: \`npm install \-D nodemon\`.

- [ ] Client: Run \`npm create vite@latest client \-- \--template react\` and install dependencies (axios, react-router-dom).

      * In the root project folder, run \`npm create vite@latest client \-- \--template react\`.

      * Navigate into the React app folder: \`cd client\`.

      * Install node modules: \`npm install\`.

      * Install routing and HTTP client packages: \`npm install axios react-router-dom\`.

- [ ] Setup ESLint and Prettier for code formatting.

      * Install ESLint and Prettier extensions in your editor (e.g., VS Code).

      * Add a \`.prettierrc\` configuration file in the project root with standard formatting rules (e.g., \`singleQuote: true\`, \`semi: true\`).

Configure npm scripts in \`package.json\` to run linter checks across workspace projects.

# **Phase 2: Backend Development (Express & MongoDB)**

- [ ] Create MongoDB Atlas cluster and get connection string.

      * Sign in to MongoDB Atlas and create a new free-tier shared cluster.

      * Create a database user with username and strong password under Database Access.

      * Whitelist your current IP address (or \`0.0.0.0/0\` during development) under Network Access.

      * Click "Connect" on the cluster, select "Connect your application", and copy the URI string format.

- [ ] Setup \`.env\` file with PORT, MONGO\_URI, and JWT\_SECRET.

      * Create a \`.env\` file inside the \`server/\` directory.

      * Add variables: \`PORT=5000\`, \`MONGO\_URI=mongodb+srv://\&lt;username\&gt;:\&lt;password\&gt;@cluster.mongodb.net/templateDB\`, and \`JWT\_SECRET=your\_jwt\_secret\_key\`.

      * Confirm \`.env\` is listed in \`server/.gitignore\` to protect API keys.

- [ ] Create \`server.js\` file and setup basic Express server with CORS and JSON middleware.

      * Create \`server/server.js\` and import \`express\`, \`cors\`, and \`dotenv\`.

      * Invoke \`dotenv.config()\` at the top of the entry file.

      * Instantiate app with \`const app \= express()\` and add \`app.use(cors())\` and \`app.use(express.json())\` middleware.

      * Call \`app.listen(PORT, ...)\` to verify server startup in terminal.

- [ ] Connect to MongoDB using Mongoose.

      * Create a database helper file \`server/config/db.js\`.

      * Write an asynchronous function using \`mongoose.connect(process.env.MONGO\_URI)\`.

      * Wrap connection attempt in \`try...catch\` block to handle connection errors gracefully, then import and invoke it in \`server.js\`.

- [ ] Create \`User\` Mongoose schema (username, password, role).

      * Create \`server/models/User.js\` file.

      * Define schema fields: \`username\` (String, required, unique), \`password\` (String, required), \`role\` (String, enum: \['agent', 'admin'\], default: 'agent').

      * Add \`timestamps: true\` schema option and export Mongoose model \`mongoose.model('User', userSchema)\`.

- [ ] Create \`Template\` Mongoose schema (title, content, tags, category, usageCount).

      * Create \`server/models/Template.js\` file.

      * Define schema properties: \`title\` (String, required), \`content\` (String, required), \`tags\` (\[String\]), \`category\` (String), \`usageCount\` (Number, default: 0).

      * Export model via \`mongoose.model('Template', templateSchema)\`.

- [ ] Build Authentication Controller (register, login, generate JWT).

      * Create \`server/controllers/authController.js\`.

      * Implement \`register\` controller method: Hash user password with \`bcryptjs.hash()\` prior to user persistence.

      * Implement \`login\` controller method: Validate user existence and compare password with \`bcryptjs.compare()\`.

      * Generate signed payload token with \`jsonwebtoken.sign({ id, role }, process.env.JWT\_SECRET, { expiresIn: '1d' })\` and send in response.

- [ ] Build Template Controller (getAll, getById, create, update, delete).

      * Create \`server/controllers/templateController.js\`.

      * Write handler functions interacting with Mongoose methods (\`find()\`, \`findById()\`, \`create()\`, \`findByIdAndUpdate()\`, \`findByIdAndDelete()\`).

      * Return structured JSON responses and appropriate HTTP status codes (200, 201, 400, 404, 500).

- [ ] Build Recommendation Controller (search templates by keywords/tags).

      * Create \`recommendationController.js\` or include search action in \`templateController.js\`.

      * Read query string parameters (\`req.query.q\`).

      * Use MongoDB \`\$regex\` or \`\$in\` query filters matching query terms against template \`title\`, \`tags\`, or \`category\` fields.

- [ ] Setup Express Routes for Auth and Templates.

      * Create route files: \`server/routes/authRoutes.js\` and \`server/routes/templateRoutes.js\` using \`express.Router()\`.

      * Create auth middleware \`server/middleware/authMiddleware.js\` verifying JWT headers (\`req.headers.authorization\`).

      * Mount routers in \`server.js\`: \`app.use('/api/auth', authRoutes)\` and \`app.use('/api/templates', templateRoutes)\`.

- [ ] Test all API endpoints using Postman.

      * Create a Postman Collection for the API endpoints.

      * Send requests to test user registration and login, saving returned JWT.

Test template CRUD operations adding \`Authorization: Bearer \&lt;token\&gt;\` in Postman request headers.

# **Phase 3: Frontend Development (React)**

- [ ] Cleanup Vite default files and setup folder structure (components, pages, services, context).

      * Delete unnecessary boilerplate files like \`src/assets/react.svg\` and clean \`src/App.css\`.

      * Create subdirectories under \`client/src/\`: \`components/\`, \`pages/\`, \`services/\`, \`context/\`.

- [ ] Setup React Router with routes for Login, Dashboard, and Template View.

      * Wrap \`\&lt;App /\&gt;\` component with \`\&lt;BrowserRouter\&gt;\` in \`src/main.jsx\`.

      * In \`src/App.jsx\`, configure routes using \`\&lt;Routes\&gt;\` and \`\&lt;Route\&gt;\` for \`/login\`, \`/dashboard\`, and \`/templates/:id\`.

      * Build a \`ProtectedRoute\` wrapper component to redirect unauthenticated users to \`/login\`.

- [ ] Create AuthContext to manage global user state and JWT token.

      * Create file \`src/context/AuthContext.jsx\` and instantiate \`createContext()\`.

      * Store user data and token state in \`useState()\`, initializing token from \`localStorage.getItem('token')\`.

      * Expose \`login()\` and \`logout()\` helper methods via Context Provider value prop.

- [ ] Build \`api.js\` using Axios to handle backend requests and attach JWT to headers.

      * Create file \`src/services/api.js\` and instantiate Axios instance using \`axios.create({ baseURL: 'http\://localhost:5000/api' })\`.

      * Add request interceptor (\`api.interceptors.request.use()\`) to dynamically set \`Authorization\` header with bearer token.

- [ ] Build \`Login\` page component with form validation.

      * Create \`src/pages/Login.jsx\` containing username and password inputs tied to component state.

      * Add submit handler calling auth API endpoint and displaying inline error messages for invalid credentials.

      * On successful authentication response, call \`AuthContext\` login function and navigate user to \`/dashboard\`.

- [ ] Build \`Dashboard\` component with layout (Sidebar, Header, Main Content).

      * Create \`src/pages/Dashboard.jsx\` using flexbox/grid layout structure.

      * Integrate \`Header\` component showing current logged-in user profile details and logout button.

      * Position search bar and recommendations feed within the primary main content area.

- [ ] Build \`SearchBar\` component to take agent queries (e.g., "customer wants refund").

      * Create \`src/components/SearchBar.jsx\` containing text input and search submit button.

      * Pass input state up to parent via \`onSearch\` prop handler or trigger query fetch on input change event.

- [ ] Build \`TemplateList\` component to display recommended templates.

      * Create \`src/components/TemplateList.jsx\` accepting array of template objects as props.

      * Map over array rendering a \`TemplateCard\` component for each individual item.

      * Render fallback empty state message if query returns empty result set.

- [ ] Build \`TemplateCard\` component to show template preview and a "Copy to Clipboard" button.

      * Create \`src/components/TemplateCard.jsx\` displaying template title, preview snippet, and category tags.

Add action button labeled "Copy to Clipboard" with click handler.

# **Phase 4: Integration and Polish**

- [ ] Connect SearchBar to backend recommendation endpoint.

      * In \`Dashboard.jsx\`, write search query function calling API service method \`api.get('/templates/search?q=...')\`.

      * Update templates state array with response results to auto-update view.

- [ ] Implement "Copy to Clipboard" functionality using navigator.clipboard API.

      * In \`TemplateCard.jsx\`, invoke \`navigator.clipboard.writeText(template.content)\` upon button click.

      * Provide visual user feedback by temporarily updating button text state to "Copied\!".

- [ ] Add simple loading spinners during API calls.

      * Define \`loading\` boolean state variable initialized to \`false\` during data fetch routines.

      * Conditionally render a spinner UI component while \`loading\` state resolves true.

- [ ] Add error handling and toast notifications for failed API requests.

      * Install notification library: \`npm install react-toastify\` inside client folder.

      * Catch API errors inside \`try...catch\` blocks and trigger \`toast.error(error.message)\` notifications.

- [ ] Ensure responsive design using CSS Grid/Flexbox or a UI framework.

      * Apply media queries or responsive flexbox/grid layout properties (\`grid-template-columns: repeat(auto-fit, minmax(280px, 1fr))\`).

Test UI layouts on mobile and tablet viewport widths using browser developer tools.

# **Phase 5: Deployment**

- [ ] Remove any hardcoded localhost URLs and use environment variables for API URL.

      * Create \`client/.env\` file containing \`VITE\_API\_BASE\_URL=http\://localhost:5000/api\`.

      * Update Axios base configuration in \`src/services/api.js\` to reference \`import.meta.env.VITE\_API\_BASE\_URL\`.

- [ ] Deploy backend to Render.

      * Push repository code to GitHub or GitLab remote service.

      * Create new Web Service on Render, targeting your repository and setting root directory to \`server\`.

      * Configure environment variables (\`MONGO\_URI\`, \`JWT\_SECRET\`, \`PORT\`) in Render dashboard settings panel.

- [ ] Deploy frontend.

      * Connect frontend project repository to deployment service (e.g., Render, Vercel, or Netlify).

      * Set build settings: Build command \`npm run build\` and output directory \`dist\`.

      * Set \`VITE\_API\_BASE\_URL\` environment variable pointing to live backend deployment URL.

- [ ] Test the live application end-to-end.

      * Access live client web app link and test user registration and login.

Perform search operations, view templates, and verify template text copies to clipboard as expected.

# **Phase 6: Automated Testing**

- [ ] Setup unit testing framework for backend controllers and middleware.

      * Install testing packages in the \`server/\` directory: \`npm install \-D jest supertest\`.

      * Configure Jest in \`package.json\` or create a \`jest.config.js\` file specifying the test environment as Node.

      * Write unit tests for authentication logic and template controllers under \`server/tests/\`.

- [ ] Build API integration tests using Supertest.

      * Set up an in-memory MongoDB server using \`mongodb-memory-server\` for isolated integration testing.

      * Write tests targeting authentication and CRUD endpoints to verify status codes and JSON response structures.

- [ ] Setup frontend testing with Vitest and React Testing Library.

      * Install dependencies in \`client/\`: \`npm install \-D vitest @testing-library/react @testing-library/jest-dom jsdom\`.

      * Configure Vitest in \`vite.config.js\` and add a setup file for DOM matchers.

      * Write component tests for \`SearchBar\`, \`TemplateCard\`, and \`Login\` pages.

# **Phase 7: Input Validation & Sanitization**

- [ ] Implement request validation middleware on Express routes.

      * Install express-validator in the server app: \`npm install express-validator\`.

      * Create validation rules for user registration/login (checking email format and password strength).

      * Add validation checks to template creation routes to enforce required fields and string lengths.

- [ ] Sanitize incoming request body payload to protect against XSS and NoSQL Injection.

      * Install sanitization libraries: \`npm install express-mongo-sanitize xss-clean\`.

      * Apply \`mongoSanitize()\` and \`xss()\` middleware globally in \`server.js\`.

# **Phase 8: Centralized Error Handling**

- [ ] Create Custom Error Handler middleware.

      * Define a custom \`ErrorResponse\` class extending the native JavaScript \`Error\` class under \`server/utils/errorResponse.js\`.

      * Create centralized middleware \`server/middleware/errorMiddleware.js\` handling Mongoose bad ObjectIDs, duplicate keys, and validation errors.

      * Register the error middleware at the bottom of middleware stack in \`server.js\`.

- [ ] Refactor async controller endpoints with an async wrapper helper.

      * Create an \`asyncHandler\` middleware utility to eliminate repetitive \`try...catch\` blocks across controller functions.

      * Wrap controller methods in \`asyncHandler\` to pass unhandled rejections automatically to the centralized error handler.

# **Phase 9: Rate Limiting & Security Headers**

- [ ] Add HTTP security headers using Helmet.

      * Install Helmet library in server app: \`npm install helmet\`.

      * Import and register Helmet middleware \`app.use(helmet())\` near top of \`server.js\`.

- [ ] Configure API rate limiting against abuse and Denial of Service (DoS).

      * Install rate limiter package: \`npm install express-rate-limit\`.

      * Define a limiter rule (e.g., max 100 requests per 10 minutes) and apply it to sensitive routes like authentication and recommendation search.

# **Phase 10: CI/CD Pipeline**

- [ ] Set up GitHub Actions workflow for continuous integration.

      * Create workflow configuration directory and file: \`.github/workflows/ci.yml\` in the project root.

      * Define jobs to install dependencies, run linter checks (\`npm run lint\`), and execute automated test suites on pull requests to \`main\`.

- [ ] Configure automatic deployment triggers.

Link hosting provider webhooks (e.g., Render, Vercel) to trigger automatic deployments only upon successful CI workflow execution on \`main\` branch.