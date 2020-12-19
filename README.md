mkdir -p ~/.aws-lambda-rie
curl -Lo ~/.aws-lambda-rie/aws-lambda-rie https://github.com/aws/aws-lambda-runtime-interface-emulator/releases/latest/download/aws-lambda-rie
chmod +x ~/.aws-lambda-rie/aws-lambda-rie
docker build -t lambda/python/norie .
docker tag lambda/python/norie:latest 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com/lambda/python/norie:3.9-alpine3.12

docker run -d -v ~/.aws-lambda-rie:/aws-lambda -p 9000:8080 \
       --entrypoint /aws-lambda/aws-lambda-rie 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com/lambda/python/norie:3.9-alpine3.12 /lambda-entrypoint.sh app.handler

curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'