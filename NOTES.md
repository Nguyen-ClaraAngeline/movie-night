#### In which file are the `<html>` and `<body>` tags?
* `app/layout.tsx`.

#### Which file renders the page at /?
* `app/page.tsx`.

#### Which folder is for images and other static files, and how do you reference them?

* The `public` folder. It is referenced as the file's name.

#### What is  tsconfig.json  doing in your project?
* It sets the TypeScript options for the project.

#### What does @/* mean in tsconfig.json? Write an import that uses it.
* It lets you import from the project root. Example: `import "@/app/globals.css";`

#### What does app/layout.tsx import from next/font/google, and what does Next.js do with it at build time?
* It imports `Geist` and `Geist_Mono`. Next.js downloads the fonts at build time and serves them with the app.

#### What’s the difference between layout.tsx and page.tsx?
* `layout.tsx` is the shared structure. `page.tsx` is the content for a specific page.

#### Where would you put a navbar that appears on every page?
* In `app/layout.tsx`, before `{children}` inside the body.

#### What is globals.css for, and where is it imported?
* It contains styles for the whole app. It is imported in `app/layout.tsx`.

#### What is AGENTS.md, and does your app need it to run?
* It gives instructions to AI coding assistants. No, the app does not need it to run.

#### In the page source, are the movie titles present for `/movies`? For `/movies-client`? Why?
* Yes for `/movies`, because the server renders the movies. No for `/movies-client`, because it loads them in the browser after the page loads.

#### Without the `force-dynamic` line of Part E, you run `npm run build` again tomorrow, then `npm start`, without changing anything else. What does the "Rendered at" timestamp of `/movies` show, and why?
* It shows tomorrow's build time, because the page is generated during the build. It stays the same when you refresh.

#### Part E made every request to `/movies` about one second slower. Give one situation where `force-dynamic` is still the right choice, and explain why.
* When showing live ticket availability, because each request needs the latest data.

#### A classmate sees `[movies] rendered 6 movies` in the browser console and says: "So this code runs in the browser." What do you answer, and how can you prove it?
* It runs on the server. Next.js can forward server logs to the browser during development. Check the terminal log, or disable JavaScript and reload `/movies`.

#### Which version would you choose for a public movie catalogue that changes once a week, and why?
* The static server-rendered version, without `force-dynamic`. It loads quickly, includes the titles for search engines, and can be rebuilt when the catalogue changes.

#### Lab 2 ####

How can `aria-current="true"` already appear in the page source if `NavLink` uses `usePathname`?
* Client Components can still be pre-rendered into HTML on the server. Next.js knows the current URL, so it renders `aria-current="true"` in the initial HTML. The browser then hydrates the component.

Why does `generateStaticParams` return `String(movie.id)` instead of `movie.id`? And why does `/movies/42` return a 404 without running the page code?
* URL parameters are strings, so `String(movie.id)` converts the numeric ID into a string. In Part C, `dynamicParams` is disabled, so Next.js only accepts the IDs returned by `generateStaticParams`. Since 42 is not included, Next.js returns a 404 before rendering the page.

Why is the note kept between `/movies` and `/movies/2`, but lost when navigating through `/about`?
* Both `/movies` and `/movies/2` share `app/movies/layout.tsx`, so the layout stays mounted and preserves the note. `/about` is outside this layout, so navigating there unmounts it and the note is lost.

In Part E, step 4, where is the real message of the error, and why does Next.js not send it to the browser?
* The full error message is available in the server terminal or logs. Next.js only sends a generic error message and an error digest to the browser to avoid exposing sensitive information about the server.

Why does "Loading the movies..." appear under `npm run dev` on `/movies/2`, but not under `npm start`, while it still appears on `/movies`?
* In development, Next.js renders routes on demand, so the loading fallback can appear while the page is being prepared. In production, `/movies/2` is statically generated using `generateStaticParams`, so its HTML is already prepared during the build. `/movies` still displays the loading message because it loads its movie list dynamically.

#### Lab 3 ####

#### After `npm run build` with the API running, the terminal of the API shows 7 calls. Which calls are they? `generateMetadata` and the page of a movie both call `getMovie`: why does the API receive only one call for each movie?
* The API receives 7 calls to retrieve the movies. `generateMetadata` and the movie page both call `getMovie`, but Next.js deduplicates identical requests during rendering, so only one API call is made for each movie.

#### In break 3, where did the value of `MOVIES_API_KEY` end up, who can read it, and where should the key be used instead?
* The `MOVIES_API_KEY` was exposed to the client, meaning users could potentially retrieve it through the browser or the client-side code. It should be stored in a server-side environment variable and used only in server-side code to make requests to the Movies API.

#### Explain in your own words why Next.js refuses a function as a prop (break 1) and a `server-only` import in a Client Component (break 2.
* Next.js refuses a function as a prop because functions cannot be serialized and passed from a Server Component to a Client Component. It also refuses `server-only` imports in Client Components because server-only code must never be executed or exposed in the browser, helping protect server logic and secrets.

#### Lab 4, part 2 ####

#### 1. Complete the table of part E: each page of Movie Night, its strategy (SSG, ISR, SSR or CSR) and why this strategy fits the page. Add the real build output next to your predictions.
Page	Strategy	Build output	Why?
/movies	ISR	○ /movies (10s)	The movie list is regenerated in the background every 10 seconds, keeping it reasonably up to date while serving cached pages.
/movies/[id]	SSG	● /movies/[id]	Each movie page is generated at build time using generateStaticParams.
/	SSR	ƒ /	The homepage needs fresh vote data on every request. Streaming allows the page to display its content before the top 3 is ready.
/movies-client	CSR	○ /movies-client (verify with your actual build)	The browser fetches the movie list after the page loads, using useEffect.

#### 2. After rm -rf .next/cache, the build makes 8 calls to the API. Which calls are they?
- 1 call to /api/movies from generateStaticParams.
- 1 call to /api/movies for the /movies page.
- 6 calls to /api/movies/1, /api/movies/2, /api/movies/3, /api/movies/4, /api/movies/5 and /api/movies/6.
The requests from generateMetadata and the movie page are deduplicated because they use the same URL and the same fetch options (cache: "force-cache").

#### 3. The home page took about 2.5 s without <Suspense>, then 2.5 s with it, then 1.5 s with Promise.all. What does each change improve: the first byte, the total, or both? Why?
- Without Suspense, both the first byte and the total response time are about 2.5 seconds because the page waits for the data before sending anything.
- Adding Suspense improves the first byte (below 0.1 seconds), because the page can send its content and the fallback immediately. The total remains about 2.5 seconds because the requests are still sequential.
- Using Promise.all improves the total time (about 1.5 seconds), because both API requests run in parallel instead of one after the other. The first byte remains below 0.1 seconds.
#### 4. Complete the table of part H: what happens when the API is down?
Page	What you see	Why, with the strategy of the page
/movies	The movie list still appears.	ISR serves the last successfully generated page from the cache.
/movies/2	The movie details still appear.	SSG generated the page at build time, so it does not need the API for each request.
/	The title and "Counting the votes..." appear, but the top 3 list fails to load.	SSR fetches fresh data on every request. The Suspense fallback is sent first, but the API request fails.
/movies-client	"Loading movies..." appears, followed by the error message.	CSR fetches data directly from the browser. Since the API is down, the request fails and failed becomes true.