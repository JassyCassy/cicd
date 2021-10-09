version = latest
account = $(shell aws sts get-caller-identity --query "Account" --output text)
repo = apiapp
target = dkr.ecr.us-east-1.amazonaws.com
namespace = wedapi-dev

build:
	@docker build -t $(account).$(target)/$(repo):$(version) .

push: login
	@docker push $(account).$(target)/$(repo):$(version)

login:
	@aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $(account).$(target)

deploy:
	@cat kubernetes/deployment-svc-api.yaml | sed "s/IMAGEVERSION/$(version)/g;s/ACCOUNT/$(account)/g;s/NAMESPACE/$(namespace)/g" | kubectl apply -f -
	@kubectl apply -f kubernetes/deployment-svc-api.yaml