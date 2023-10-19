Flux has it's own UI with vscode and weaveworks
Basically gives you a folder tree for the Flux objects and their status and easier ways to make new objects. 
VSCode one has a plugin you can install. Not using it so I won't bother. 
For weave:
```sh
curl --silent --location "https://github.com/weaveworks/weave-gitops/releases/download/v0.34.0/gitops-$(uname)-$(uname -m).tar.gz" | tar xz -C /tmp
sudo mv /tmp/gitops /usr/local/bin
gitops version

gitops create dashboard ww-gitops --password=[pass] 

k -n flux-system get svc #will have to expose ww-gitops service
k -n flux-system edit svc [name]
```
Can't add things with weave UI. 
![Pasted image 20231019092610.png](<Pasted image 20231019092610.png>)