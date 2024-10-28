# Checking if a token is valid

## Florian_Link on 2019-04-04T18:09:53.655Z

I have a backend websocket server that only wants to start expensive operations of a request has a valid user session token from girder.  

My backend server expects to have a girder API url that returns success if the token is ok and failure if it is not valid.


I only found:


api/v1/token/current


and


api/v1/user/me


which both return a JSON response if the passed token is valid. BUT, in case of an error, they only return a “null” JSON. Of course I could change the backend server to look for a response !\= null,  

but to me it would make more sense to return an HTTP error code, which is more failsafe to check than a JSON. (The backend uses the token check not only with girder, so an error code works better for any token check API).  

I there something I have overlooked?


---

## Zach_Mullen on 2019-04-05T12:53:35.896Z

In fact, any endpoint decorated with `@access.user` will provide the behavior you want. You could add a trivial endpoint via a plugin that would do that:



```
@access.user
def validate_user(**kw):
    pass

```

That would yield a 200 response if the token is valid, and a 401 if not.


---

## Florian_Link on 2019-04-05T14:19:46.243Z

Thanks! I found that the api\_key/listKeys has such a decorator, so I can use it (although this transfers the list of api keys which I don‘t need).


Would you mind adding such a validate route to the user.py api?  

I know it is easy to add own extensions, but it is simpler to just use the official girder container.


An alternative would be a token/validate with public access and token as parameter?


---

## Zach_Mullen on 2019-04-05T14:32:40.793Z

Since it’s a small amount of code, I’d say it’s worth putting in a PR for such a core endpoint, though I can’t speak for how everyone would feel about it. I’d personally be OK with it.


---

