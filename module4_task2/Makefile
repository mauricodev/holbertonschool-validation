# Makefile Local
.PHONY: build

# Destiny directory
DIST_DIR = dist

build: ## Create the Hugo website using Hugo
	@sudo apt-get install curl -y
	@sudo curl -L https://github.com/gohugoio/hugo/releases/download/v0.109.0/hugo_extended_0.109.0_linux-amd64.deb -o hugo.deb
	@sudo apt-get install ./hugo.deb
	@sudo rm ./hugo.deb
	@hugo
	@touch awesome-api

clean: ## Remove the directory of the result code of Hugo website
	@rm -rf $(DIST_DIR)

NEW_LINE = title: \"$(POST_TITLE)\"
post: ## Create a new post in Markdown to content/posts using POST_NAME and POST_TITLE environment variables for filename and title respectively
	@rm -rf content/posts/$(POST_NAME).md
	@hugo new posts/$(POST_NAME).md
	@sed -i '1,2d' content/posts/$(POST_NAME).md
	@sed -i '1i ---' content/posts/$(POST_NAME).md
	@sed -i '2i $(NEW_LINE)' content/posts/$(POST_NAME).md

package: ## Create a zip file with dist/ directory
	@sudo apt-get install curl -y
	@sudo curl -L https://github.com/gohugoio/hugo/releases/download/v0.109.0/hugo_extended_0.109.0_linux-amd64.deb -o hugo.deb
	@sudo apt-get install ./hugo.deb
	@sudo rm ./hugo.deb
	@hugo
	@zip -r awesome-website.zip dist/

lint: ## Lint the README.md and DEPLOY.md files and lint Dockerfile
	@sudo apt-get install curl -y
	@sudo curl -fsSL https://deb.nodesource.com/setup_16.x | sudo bash -
	@sudo apt-get install -y nodejs
	@sudo npm install -g markdownlint-cli -y
	@sudo markdownlint README.md DEPLOY.md
	@sudo curl https://github.com/hadolint/hadolint/releases/download/v2.12.0/hadolint-Linux-x86_64 -o hadolint
	@sudo mv hadolint /usr/local/bin/
	@sudo chmod +x /usr/local/bin/hadolint
	@sudo hadolint build/Dockerfile && echo OK

unit-tests: ## Unit tests coverage
	@touch coverage-units.out

integration-tests: ## Integration tests coverage
	@touch coverage-integrations.out

build-docker: ## Create a new Docker image named awesome:build
	@docker build -t awesome:build -f build/Dockerfile .

docker-build: ## Create a new Docker image named awesome:build passed lint
	@sudo wget -O /usr/local/bin/hadolint -q -nv https://github.com/hadolint/hadolint/releases/download/v2.12.0/hadolint-Linux-x86_64
	@sudo chmod +x /usr/local/bin/hadolint
	@sudo hadolint build/Dockerfile && docker build -t awesome:build -f build/Dockerfile .

makefile-docker: ## Prepare correct Makefile for Docker
	@cat Makefile > MakefileLocal
	@cat MakefileDocker > Makefile

makefile-local: ## Prepare correct Makefile for local
	@cat Makefile > MakefileDocker
	@cat MakefileLocal > Makefile

run-docker: ## Run docker
	docker run -it --name alpine-go -v $(pwd):/app awesome:build /bin/sh

test: ## Execute Tests
	@make unit-tests
	@make integration-tests

docker-tests: ## Test of Docker with container-structure-test command
	@sudo curl https://storage.googleapis.com/container-structure-test/latest/container-structure-test-linux-amd64 -o container-structure-test
	@sudo mv container-structure-test /usr/local/bin/
	@sudo chmod +x /usr/local/bin/container-structure-test
	@sudo container-structure-test test --image awesome:build --config ./build/cst.yml

validate: ## Validate project

help: ## Show usage of each make target
	@echo "Usage: make <command>"
	@echo "------"
	@fgrep -h "##" Makefile | fgrep -v fgrep | sed -e 's/\\$$//' | sed -e 's/##//'