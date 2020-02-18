whatdandoes.info
================

I am a Wycliffe Bible Translator. This is my missionary blog.

# Set up

```
npm install
```

# Build static static

```
npx hexo generate
```

# Create new post

```
npx hexo new post "Post title"
```

# Development server

```
npx hexo s -p 3001
```

# Production

This is meant to be deployed behind a Dockerized `nginx-proxy`/`letsencrypt-nginx-proxy-companion` combo:

```
npm install
npx hexo generate
docker-compose up -d
```

