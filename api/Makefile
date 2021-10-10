account = $(shell aws sts get-caller-identity --query "Account" --output text)
repo = apiapp
target = dkr.ecr.us-east-1.amazonaws.com
stage = dev
include stages/$(stage)

build:
	@docker build -t $(account).$(target)/$(repo):$(version) .

push: login
	@docker push $(account).$(target)/$(repo):$(version)

login:
	@aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $(account).$(target)

deploy:
	@cat kubernetes/deployment-svc-api.yaml | sed "s/VERSION/$(version)/g;s/ACCOUNT/$(account)/g"
	@kubectl apply -f kubernetes/deployment-svc-api.yaml