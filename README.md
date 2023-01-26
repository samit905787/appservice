# appservice
docker buildx build --platform linux/amd64 -t bhashini.azurecr.io/bhashini:3 .

docker run -d -p 8096:5000 --rm --name mypythonContainer ${registryUrl}/${registryName}
