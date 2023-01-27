# Prisma data proxy issue

1. set `DATA_PROXY_DATABASE_URL` env pointing to dataproxy prisma connection
2. `npm i` will run `npx prisma generate --data-proxy`
3. `npm run dev`
4. go to `http://localhost:3000/`

You should see the error produced by prisma:

```
Cannot fetch data from service:
include is not a function
```

## Possible fix

To hotfix the issue:

- go to `/prismaClient/runtime/index.js`,
- move following piece of code (line around 23971)

```
__name(nodeFetch, "nodeFetch");
var include = typeof require !== "undefined" ? require : () => {
};
```

above the `nodeFetch` function

```
__name(buildResponse, "buildResponse");
async function nodeFetch(url, options = {}) {
const https = include("https");
const httpsOptions = buildOptions(options);
```

where the `include` function is being called.

### Note:

I am committing also the prisma generated output, which probably won't be part of the real application.
