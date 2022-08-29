## How to build scalable project with monorepo

There are many ways to create and mantain software projects. If it comes to the code respository itself and code managing a classic approach is to create one repository per each project and it worked that way for many years. But what if the code we write in one project can be used in other projects as well? Or if the logic for frontend and backend code is duplicated and consists of the same rules and statements? Monorepos come to the rescue.

## What, why and when

Monorepos are modern approach for building and maintaining complex apps, which consist of multiple layers and shared logic. Almost every bigger company uses monorepos in one way or another, let it be Google, Facebook or Airbnb. Monorepos are widely used in popular javascript libraries like Jest, Create React App or React Query (nowadays known as Tanstack Query). Google codebase is speculated to have 80 terabytes of raw code, which is held in one big repository mantained by [Bazel](https://bazel.build/)

## Pros & cons

The main advantage of monorepo is we have one source of truth. Instead of having a lot of repositories with their own codebase and configs we have single repository, enabling us for code sharing and reusability. Monorepo also simplifies dependency management as we can have single configuration file for all the projects within it. As with everything monorepos also have their disadvantages. Main one is code discovery - with growing codebase there will be a point in time where searching for code for reuse will be much harder due to codebase size itself. Another disadvanatage is access restriction - every git service provider or git itself provides access on repository level which means every member who has access to the repo will have access to the whole codebase.

## Monorepo tooling

Quick google search will provide us bunch of results regarding monorepos. We have yarn workspaces, rush, lerna, bazel, turorepo or nx to name some of them. We will focus on the last one: nx

Nx is developed by Nrwl and started as pure monorepo tool, today it evolved more to be a build system and provides bunch of tools to structure our code. It helps to build, test and deploy apps. It has support for Typescript, React, Angular, Nest.js, Next.js and many more. If it doesn't support technology out of the box we can utilize generators which can be configured to build any type of project. On top of that nx has great CLI which can speed up development and deployment greatly.

Nx also utilized caching and robust dependency management, which speeds up builds and deployments. Nx builds a graph of dependencies and rebuilds only parts of the monorepo which were affected by changes in some cases reducing build times by over 70%.

## Let's build a monorepo

Let's jump into some examples. Let's say we want to create some frontend and backend services using nx and utilizing next js for frontend and nest js for backend part. We can create our workspace with one command:

![create monorepo](https://lh3.googleusercontent.com/2CwZ_8FHRRfd5xznPjhfyzqoBbVC8CVRdAz4HeCE0IX7PCoQop4GfEj4BvU4Z0VoskqTOruTnGON79X7xKuGhYxsgh90p6IzFIq10luo6wxsaoCtKuo8k_0Kb9Ws_o2ue78lNyQEoA2dNok1BAdKRwu_8J3CC8c_I1S6Rbu1nLixXlsrPJtQwflcDBSRY-2dtwhnvZEBEmYy-uYRlPi39hTRCJiTSsmaW3_tsoCZVsMGdbtMVf2zp7co_1asNNgQve4rMp8oQ1vkv1Yefw-o3ZDEzpYXEcDdMIZoO-5g5DpyenfAiYtLrlh18MC2AdfCCSxULxO8DhtBsCBd4nmSdWYd9iKRTIpwr8qAdCEknl8KvmqOWubH9DZYz2qbgQblQygVSs8mSVc6p2WV5ijXsVBFTlbWUca5iRXSvGLLntTVAJkGbEKlhjBSK_JfCfMsnJJw-iXgnmh0N65DszOBlIn-iy7blK2-jj1G-vRbhr0fqnKVIMU--USluDgUwwCL9E9OhKnugGGnBqnURYBf1jvoh-RTzrPI3T6zjSdfTibInF3AZZWspCc4bkJZiU7L2XdMCA8WAxsg11obhMf1XoDiPFUlOxME3dDH0v6u4_wFW1f5qUyhO-mvsWHe_8XygenFS5m2qHb4sCMurN5Pw1EFdKOLcLukgSv160malXip_zkoQWWfeiA69vVKSMN6uY2FrNB-QCHCUQWZqTrShcT5HZJJaMeAmlYjqFEBaNu6LngnSuvSbr1zzW0ro104X3zXbsQRpljU9vOoOT9LCVRJ8mXzpgzy70o=w806-h410-no?authuser=0)

Nx will prompt us for a workspace name, workspace type (can be raw workspace or with settings for frontend apps, backend apps or even npm libraries)

Then to create Next.js app:

![create next](https://lh3.googleusercontent.com/hPg20zBd_GpBiTiqx0NeKzAaa0QEyYaJPG7c2v0ZQPEM4hP6K9hGgnfxoSoDRRFcEO3GAzkoBknjUgazrEBw8U_2ma1hExeRwVwudj5L3khm6MWl26o-kV_ssCqnZf0n3OlZ6v8LMdE983iZZTVaMNlc7732kjVeF2bjOMJMRaTKYnlg3o6yT53Sx8-plYmVuSLdmOMGLnd04NvjWW3cTHsYCPPOxi6WVdspwPgrc1hlxwIuEvEDRyMhgnHono5lgxBDVIiHjR6G4mHVNrlxOjP8oge2KoXXZJDy8iu-0x76Tusth03TRqBeNBbzesK2o_kGPgVkTeKR-OmTOop40Ge85wauwg67P78sQbRPMp2DUGKbGiHuV6kWOYlEBu5xxTF6vXId9vtjUZNhQQjAc850M16vrKd76rGaqcGb-wuA-OmnG9oXV1MocHf8tTqGdtm8vtkeLcvZx_729X0CT8KSPbvADjZTjJdygxua1-o1AdsbCL6aemWdGKwgq31cm5mAktE1AC5iFjRQ93dKrH8rtuZ-zTacL7VEduXOZUSz4XlgQp-08zjne7wTXbsqouzFs1TJYbk_NautsVShviaoJrFHtJL9wF-aVoyn5lUf6yAcoHJcQqTySqeV2tbjMR-ypotsHg3iz2hiqmGarz3EHCmVHfNCzA8b1HYHMmN9T0vSSljqoBfxQkSYbDGXVgjuchZAcc0VR1ugDYJ5ir3iI4InrV4PlGFsbN0w6x6SCb5ojMERbFq6iYjzN4JR0ymqNxkqSYnfFHFoTAyb3VwYlMpFINcTHd0=w798-h416-no?authuser=0)

And Nest.js app:

![create nest](https://lh3.googleusercontent.com/GGHZJZwAX6dn3OmS8c7YtH9GX4VPemWCTmS-3CQ6v-Srfx1r33Jum0LjpE_MQceixcDma9yaBfxte1KQlT5KKL1J32YCALty743Y0FQtQZgNwPm0LGcggSO5bbqjLrJ0G6BLtG0DZexIl7zDynpMtsRfssiZmGq92xPYE2TU_oYJ_atKxfQ67SLyYjqotMwe5oWPZY-4WBvsVLQXtzMogbyjkQrwbaVaLIdoygsHr26FxJeam7NDYIjUqDWJw8h7M7vUwIj5gU51LF37L3vP599bY_wo9kGBRME55Cx_wgvFvwETPMZmQAEL3ILeA0gnOu8y_W3C_oAfZZo7h8XoE7aIrdSk4kUP6g7xeR4JgDmrHaJR-Fw55Pmegi7kYdNTuTdUJQdGZYyqzJ0N9BEBcXF-xkDT2FqutvH85gDulDkGppRiACoZsxIF4Y4PuSGGuTfnBs4zsqhodeGYQLqZG9bWkxkGAU5fV3ObP2KCEF8H6hoONUDtK7HsoYd0MLvPIz1w81Fv-KX27YFusuk823h2UnHruvL8taEEkmhLyvBLOHW6nGX4DAa0dbJrcNcuoZfM8tMneJuIkMLBvWEj5xscjHroiWwIg8wMNYBvBBy0CTM5VZ2W3GQeLxghvexVs3_IWvO0N_r7bFqDiyT7fLInxvxPflClTgAw4OX403QD6fWmMNQ3qQU_V7fzvRambLK9WE2d6mKaR8LGio2BvT1d6JTeoV72o0KVbitjFpCIaF7mggkoGwZQgKJvtPqOQvWI7I93zyUyCbn-ulsXwb1NkW2VSAMhrfo=w670-h333-no?authuser=0)

Depending on the app and library used nx will prompt us for different things, for example next js app will ask us for styling we want to use with the frontend, we can choose from pure css, less, sass, styled components and more. Nest js will ask us for typescript support and so on

Using Nx.js we don't have to worry about unit tests or e2e tests configuration, it is wired in automatically by the library and is ready to use and write more tests straight ahead.

Last but not least, Nx CLI provides us with bunch of commands useful for testing, build or even app deployment. To run app in development mode all we have to do is:

![serve](https://lh3.googleusercontent.com/RaJHQqe9o8O-2YlhA2o4FU8PDZKQ4-fm2EDKaF_oDjOBzklpNZdT5Yz1qE3_3xzG2_zthXr7AFkxZ-Ikv14r8BeMHg9I4qgvyYp-hp4B0jYxFuf6WRimRtu-eB2JKmuTwssskH-5khG0raaQxCnKU6EcYawExHUhIPO3TCmLwEprsfuJXnZWteimaTyJXpmibOIAuP6_ZdOV2Weqr80P2Wz83FhFWlop9nLt0R8RhhqqgEqqCXLZ5gMHpvSy9eoTs2MjglElaMBAMm3tXDk3vfxiekBFg7p-aLHsbFpcqUJIDgsRdfuDwwanZaXFMf2ilwzL1MY6O2U28Hh0_agusY6TmjCK4LjHVZD6qV7KQlIqhTEiI9EyPlPb15_Ln-1lysr5PKrkrBqIFC-Hdql-UZHYFc6ZJUmYyJaP6NqTwWlNLB7ioJYSpwCGsIKsfNAT68t0w_3GhrQvTJU4g0BUfUSzvZMBN4iHv2bCJ8sFNDnGNqYHW5InqjVT_4neYkFBKFzYVZQOVk6Y4XWgBDfUPB0l3C5U3OdNYRH0m8l0traLLiJi-6LFiAIuuVXnbpYieZAVQbqOiJfMm2eXdp66GbdXzwBoawXr1gC8gB37deetIFTSjh-tB-NX1PyxjzrsNKrQPSeHRYhuclURjkgfZWVTdnrBuucFzOHTIZh6xjVHwfGUABGY3Bm6atY0z5-q1buvqotQo9E4BNvn74PqPGAsM_o3aK3PSIejS7L3aDyBRxWyX3_Df85IcUdDCnb4OFu0IkA1fo5oRlC2VLyT80XbQKhyBpMyIbE=w554-h410-no?authuser=0)

This will conclude the idea of monorepo and why we should or should not use it. Many companies use monorepos for their codebases and they have plethora of use cases. Nowadays there are far more pros than cons regarding monorepo usage.
