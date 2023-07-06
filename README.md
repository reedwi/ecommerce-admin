# E-Commerce Dashboard
Fully inspired by the [following repository](https://github.com/AntonioErdeljac/next13-ecommerce-admin/tree/master) and [video](https://youtu.be/5miHyP6lExg)

Skills and tech involved
* React
* Next
* Prisma
* Planetscale
* Clerk
* Tailwind
* MySQL
* shadcn

My video review of project [here](https://youtu.be/qHp3Xucxhog)

Need to come back to this and update the readme to explain the project, but have the video walkthrough of thoughts for now.

[Store](https://ecommerce-store-git-master-reedwi.vercel.app/) 

[Backend Admin](https://ecommerce-store-reedwi.vercel.app) (I think anyone can login technically)

## Structure things
a route inside of (dashboard) like [storeId] is a convention in next saying that this route requires there to be a storeId parameter 

## Learnings
`npx prisma migrate reset` to delete the database and `npx prisma generate` to create the types based off of prisma.schema file. `npx prisma db push` to create the schema in our connected database

In this example I am creating a store and then immediately sending the user to the store that has been created.
```window.location.assign(`/${response.data.id}`);``` will do a complete reload of the page and ensure the database row has been committed completely. Using the next router redirect there are some cases where the database row hasn't been committed

### Hydration/Server Side Window Thing
```
export const useOrigin = () => {
  const [mounted, setMounted] = useState(false);
  const origin = typeof window !== "undefined" && window.location.origin ? window.location.origin : '';

  useEffect(() => {
    setMounted(true);
  }, []);

  if (!mounted) {
    return ''
  }

  return origin;
}
```
This function, `useOrigin`, is a custom React Hook that is used to get the origin (the scheme, hostname, and port of a URL) of the current window location.

Let's breakdown its behavior:

1. It initializes a piece of state, `mounted`, to `false` using the `useState` hook. This state is used to track whether or not the component has mounted yet.

2. It then defines a variable `origin` which checks if `window` is defined (since `window` is not available during server-side rendering) and whether the `window.location.origin` exists. If it does, it assigns the origin to the `origin` variable, otherwise, it assigns an empty string.

3. It uses the `useEffect` hook to set `mounted` to `true` after the component is first rendered (since the empty dependency array `[]` means the effect runs once after the initial render). This is done to ensure that this hook only returns the origin after the component has mounted and `window` is available (which prevents issues during server-side rendering).

4. If the component has not mounted yet (`mounted` is `false`), it returns an empty string.

5. Once the component has mounted, it returns the `origin`.

So in summary, this function is a way to safely access the `window.location.origin` in a React component, ensuring that there are no issues during server-side rendering where `window` is not available.