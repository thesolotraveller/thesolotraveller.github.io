---
layout: post
title:  "Focusing on the requirements"
date:   2022-04-14 10:00:00 +0530
categories: Tech
permalink: /posts/tech/focusing-on-the-requirement
---

I started working professionally in the mid of 2017 and shortly after that, maybe 3 months later, I felt a desperation for starting a tech blog. I started looking out for how can I build one and one day I stumbled upon this cool `WYSIWYG` editor which goes by the name [react-draft-wysiwyg](https://www.npmjs.com/package/react-draft-wysiwyg).

**What to build**

I concluded that I needed an admin UI (CMS) using which I can keep generating blogs and another (readers facing) UI for listing blogs. So finalised at building a web app with a common server and a couple of UIs. At that time, I was working as a full stack engineer and my work days revolved around spending time tinkering with `NodeJS`, `ReactJS` and `PostgreSQL`. So by default I chose to have:

- NodeJS for backend
- React for a web UI
- PostgreSQL for DB
- [react-draft-wysiwyg](https://www.npmjs.com/package/react-draft-wysiwyg) for the core editor

> I had clearly drifted from the primary goal of writing blogs to building a platform for writing blogs. Even I was clearly aware of this but the dopamine hit on the idea of building something end to end overshadowed the idea of using already existing stable and popular tools like `Medium`.

**Development**

Fast forward around a month, after slowly learning about the tools I chose and iterating over what to build, I finally had the web app running on my localhost.

> What a feeling it was!

At the feature level, after landing on the homepage I could enter a name for the blog, select one of categories, add a thumbnail and on submit could see this new blog show up in a table with edit and delete buttons. Testing of the content writing and editing features was limited to `hello world` or a `lorem ipsum` at best. Attaching a few screenshots of the main user interfaces.

- `User facing blog listing screen`

##### **Using dummy data below -> `Generics` in Go were not approved back then, not even proposed I think.*

![User facing blog listing screen](/assets/bloglisting-1.png "User facing blog listing screen")

- `Blog editing (admin) screen`

![Blog editing (admin) screen](/assets/editor-1.png "Blog editing (admin) screen")

- `Blog management (admin) screen`

![Blog management (admin) screen](/assets/cmsbloggenerator-1.png "Blog management (admin) screen")

**Deployment**

Now that things were looking good at the local machine it was the time for deployment. Bought a domain `(www.mohit.work)` and after spending around a week, deployed this app on `AWS` using `RDS` and `EC2` instance, thanks to AWS's free usage for a year. For `SSL` certificates, used `LetsEncrypt`. Deployment was taken care off and the first production application that I built, out of work, was live.

**Follow Ups**

Fast forward in the span of next 2 years, I wrote just 2 blogs and whenever I spent time on this application it was mostly about making optimisations in the web application and hardly about actually using it. Some of them were:

- Terminating the usage of `RDS` and having the Postgres DB inside `EC2` instance only. This reduced my cost as free tier was over.
- Migrating the Node backend code from `Javascript` to `Typescript`. It had become quite a thing by then.
- Incorporating `code-splitting` at the client side and some other optimizations to reduce JS bundle size and load time.
- Incorporating `redux` for state management. Now when I look back, feel that redux was an overkill my use case.

So clearly I had made it my learning playground which I was totally conscious about and cool with but had realized that management, of the app as a whole, had become challenging and time consuming. Also, it sucked at the `SEO` front because of `client side rendering` so another bummer was that blogs would never be discoverable. I didn't factor this in while picking up the tools a couple years back. Finally parked this project.

Fast forward around two and a half years, which is basically now, I again decided to start a blog. This time I decided to keep things simple and went for using already available free tools and here I am with my first blog which, I know, would be discoverable during the search and will be there over the internet for as long as `Github Pages` exist.

**Ending Notes**

I had drifted from the core requirement and lost focus right after I started working on the blog but looking back I don't regret doing any of it though because it helped me better my skills at different fronts. Keeping learnings from the previous experience on the table, this time I tried to not lose focus on the requirements and will try to not let go of this learning in future.

> Thanks for taking the time to read this far. Hope there was some takeaway for you as well.
