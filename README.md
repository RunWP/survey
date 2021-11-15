# **Survey v0.1.0**

### *VueJS (educational project)*

A mini survey and statistics project on one page

## **Project setup**

#### *Install app dependencies*

```sh
npm install
```

#### *Runs the app in the development mode*

```sh
npm run serve
```

Open [**http://localhost:8080**](http://localhost:8080) to view it in the browser

#### *Builds the app for production to the `dist` folder*

It correctly bundles project in production mode and optimizes the build for the best performance

```sh
npm run build
```

## **Docker Desktop project deployment** *(Production only)*

#### *Builds image*
```sh
docker build -t survey_node:lts-alpine .
```

#### *Container instantiation*
```sh
docker run -d --name survey_ctn -p 127.0.0.1:80:8080 survey_node:lts-alpine
```

Open [**http://localhost**](http://localhost) to view it in the browser
