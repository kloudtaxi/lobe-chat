# Getting Started - Sovera-LobeChat



> This is a fork of [Lobe Chat](https://github.com/lobehub/lobe-chat.git) for integration with Sovera Platform. Anything in this branch should be considered as `out-off-tree` from the `lobe-chat origin`. 

> :pray:  As always please don't leave any **API KEYs, Tokens, Credentials** or other sensitive information in this file. 

> :reminder_ribbon:  **TODO**
>
> - [ ] Implement pre-commit hook to reject files with sensitive information. 

---

## Cloud Deployment - NON PRODUCTION
### Bootstrap Env

1. Create VM with IaaS provider of choice
2. Set SSL auth to PK only
3. SSH into VM 
      ```zsh
      ssh into the vm
      ```
4. Update OS and utilities 
   ```bash
   sudo apt update && sudo apt upgrade
   ```
5. Reboot
   ```bash
   sudo reboot
   ```
6. Install docker 
   ```bash
   mkdir temp && cd temp
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   ```

7. Create project directory 
   ```bash
   cd ~
   mkdir sovera && cd sovera 
   ```
   
8. Deploy docker proxy
   ```bash
    mkdir docker-proxy && cd docker-proxy
	touch docker-compose.yaml
9. Update `docker-compose.yaml` with this content 

   ```yaml
   version: '3.8'1. 
      services:
        docker-socket-proxy:
       image: tecnativa/docker-socket-proxy
       container_name: docker-socket-proxy
       restart: unless-stopped
      environment:
         - CONTAINERS=1
       volumes:
         - /var/run/docker.sock:/var/run/docker.sock
       ports:
         - "2375:2375"
       privileged: true
   ```



### Setup Sovera-LobeChat

1. Clone Repo

   ```sh
   git clone https://github.com/kloudtaxi/lobe-chat.git
   ```

2. Check out `sovera-lobe-chat` branch

   ```
   ```

   

3. Run docker compose file from sovera folder

   ```sh
   cd ~/sovera/lobe-chat/sovera
   docker compose up -d
   ```

4. Verify everything is working 

   ```sh
   docker compose logs -f
   ```

5. In dev or demo environment create a `.env` file in `lobe-chat\sovera` folder with `ACCESS_CODE=xxxxxxxxx`. Users will need this code to use the chatbot. 

6. Update settings in either UI or configuration file to proxy OpenAI api calls via `llama-gateway` 

   Snippet of configuration file :heavy_exclamation_mark: line 3 & 9

   ```json
      "languageModel": {
                   "openAI": {
                       "OPENAI_API_KEY": "sk-......",
                       "models": [
                           "gpt-3.5-turbo-16k",
                           "gpt-4"
                       ],
                       "customModelName": "",
                       "endpoint": "https://llama-gateway.example.com"
                   }
               },
   ```



- [ ] **TODO:** set default OpenAI proxy end point via `.env` 
