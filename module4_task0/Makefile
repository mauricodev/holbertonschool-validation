# Destiny directory
DIST_DIR = dist

build: ## Create the Hugo website using Hugo
	@hugo
	@touch awesome-api

clean: ## Remove the directory of the result code of Hugo website
	@rm -rf $(DIST_DIR)
# @find $(DIST_DIR) ! -name 'awesome-website.zip' -type d -exec rm -rf {} +

NEW_LINE = title: \"$(POST_TITLE)\"
post: ## Create a new post in Markdown to content/posts using POST_NAME and POST_TITLE environment variables for filename and title respectively
	@hugo new posts/$(POST_NAME).md
	@sed -i '1,2d' content/posts/$(POST_NAME).md
	@sed -i '1i ---' content/posts/$(POST_NAME).md
	@sed -i '2i $(NEW_LINE)' content/posts/$(POST_NAME).md

package: ## Create a zip file with dist/ directory
	@hugo
	@zip -r awesome-website.zip dist/

lint: ## Lint the files README.md and DEPLOY.md with markdownlint command line
	@markdownlint README.md
	@markdownlint DEPLOY.md

unit-tests: ## Unit tests coverage
	@touch coverage-units.out

integration-tests: ## Integration tests coverage
	@touch coverage-integrations.out

validate: ## Validate project

help: ## Show usage of each make target
	@echo "Usage: make <command>"
	@echo "------"
	@cat Makefile | grep "^\w*:.*##.*" | sed 's/## //'

