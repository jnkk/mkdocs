> [!NOTE] Note for Commands  
> `npx tailwindcss -i src/static/tailwind/tailwind-input.css -o src/static/css/cfdhome-ui.css --watch`  
> The command above is inside the `scripts` function inside `package.json`,
> is for "syncing" the tailwindcss "css site builder". Meaning run that command and it will generate code inside the `src/static/css/cfdhome-ui.css` file.
>
> `python manage.py runserver` is for running the django server.  
> For "collecting" the static files and putting it in production is using: `python manage.py collectstatic`
>
> Learn the fundamental principles of NODEJS. Like the `scripts declarations`.  
> Can run scripts in `npm run dev`.  
> The `npx` command is for executing the npm module(s). Like the Quartz for the blog in Github Pages.
>
> [Video reference of this app](https://www.youtube.com/watch?v=lsQVukhwpqQ)

# "Main Django file" is in the src

This is a page for my notes learning django and tailwindcss.
Might give [Astro Build](https://astro.build/) a try.

## Everything is in the [documentation](https://docs.djangoproject.com/en/5.1/)

Don't have time to read all of the documentation.  
It's [learn by doing](https://www.youtube.com/watch?v=lsQVukhwpqQ) django + [tailwindcss](https://tailwindcss.com/).

## Static file structure

`src/templates` can be used for better management in static files.  
Customize and managing those files can be easy to understand and to look at.

## [Tailwindcss](https://tailwindcss.com/docs/installation) can be "easy."

There are a lot of components in a website to make it look pretty. Not including how the page are "routed."  
`Route/routes` I think is a stack build jargon on how homepage include blog, about page, company page goes where and its relations in the background. In short term, hyperlink.  
Tailwindcss is a tool for how a website is viewed, not like the 2000s internet days.

This is just how the layout is built, not including fonts.

### GITIGNORE

The `.gitignore` file can be found [here](https://github.com/github/gitignore)
