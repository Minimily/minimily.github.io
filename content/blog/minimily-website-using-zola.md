+++
title = "How We Created Minimily's Website Using Zola and Publishing to Github Pages"
date = 2025-12-29
updated = 2025-12-29
description = "Description"
+++

## Creating the site with Zola

The first step is to create a content project with Zola. This is important because that's what we need to test the rest of the steps. It doesn't need to be a final version of the site, but it needs to have a theme and some targeted configurations.

As we mostly work in the Rust ecosystem, we build Zola from source using Cargo:

```cmd
$ cargo install --locked --git https://github.com/getzola/zola
```

We then use it to create a new Zola site:

```cmd
$ zola init minimily.github.io
```

It will ask a few questions to get started. That's how we answered:

* What is the URL of your site? (https://example.com): https://www.minimily.com
* Do you want to enable Sass compilation? [Y/n]: Y
* Do you want to enable syntax highlighting? [y/N]: y
* Do you want to build a search index of the content? [y/N]: y

But that's not a big deal because any choices made can be changed by modifying the `config.toml` file later.

A minimal site was created and we can move into the directory and use the built-in server: 

```cmd
$ zola serve
```

## Website Git Repository

Once the Zola project is working locally, we need to version it with Git and push the code to Github. We first initialize a local repository:

```cmd
$ git init
$ echo "public/" >> .gitignore
$ git add .
$ git commit -m "Initial commit"
```

 Then we create a public repository named according to this convention: "\<organization\>.github.io", which in our case is `minimily.github.io`. Now we just sync the local repo with the remote one:

```cmd
$ git remote add origin git@github.com:minimily/minimily.github.io.git
$ git branch -M main
$ git push -u origin main
```

## Github Pages

Let's create an API access token to allow the Github Action to access the website repository:

1. Click on your user icon, located on the top right, to see the user menu, then select "Settings"

2. On the left menu, go all the way to the end of it and click on "Developer settings"

3. On the left menu, click on "Personal access tokens", and then "Tokens (classic)"

4. Click on "Generate new token" and select "Tokens (classic)"

5. Write a note explaining the usage of the token, define the expiration time based on your security policy, and under the "Select scopes" section, select `repo / public_repo` permission and click "Generate token".

6. Copy the generated token to be used in the sequence

Let's create a "Repository variable" with the token to use in the Github Action script:

1. Go to the website repository and click on "Settings"

2. On the left menu, under "Security", click on "Secrets and variables" and then "Actions"

3. Click on "New repository secret", then give a name to be used in the script, all written in uppercase, paste the generated token in the 
"Secret" field, and click on "Add secret".

## Custom Domain

Once everything works well with the Github provided URL, it is time to define a custom domain. Before making any changes to Github Pages configuration, we have to do two things:

1. Verify the ownership of the domain

2. Configure the domain

To verify the domain:

1. Go to the Organization's Settings

2. On the left menu, select "Pages"

3. Click on "Add a domain", type the apex version (e.g: example.com), and click on "Add domain"

4. Go to your Registrar account and add a new entry to the domain DNS:

| Type  | Name                             | Data                            |
|-------|----------------------------------|---------------------------------|
| TXT   | _github-pages-challenge-Minimily | 36e3ae32f44c9283732661a38bce685 |

5. Go back to Github and click on "Verify"

To configure the domain, go to your Registrar account and add the following entries to your domain DNS:

| Type  | Name | Data               |
|-------|------|--------------------|
| A     | @    | 185.199.108.153    |
| A     | @    | 185.199.109.153    |
| A     | @    | 185.199.110.153    |
| A     | @    | 185.199.111.153    |
| CNAME | www  | minimily.github.io |