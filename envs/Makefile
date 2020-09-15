APP=example
REGION=ap-northeast-1
STAGE=dev
PROFILE=default
AWS_DEFAULT_PROFILE=${PROFILE}
BUCKET_TFSTATE=${APP}-${STAGE}-tfstate
VALIDATE_CD=([[ ! -e ${STAGE} ]] && echo 'No such stage:${STAGE} exist.' && exit 1) || cd ${STAGE}

.DEFAULT_GOAL := plan

init_tfstate_s3:
	@${VALIDATE_CD} && \
		if aws s3 ls | grep ${BUCKET_TFSTATE} >/dev/null; then \
			echo '> Already bucket exist'; \
		else \
			aws s3 mb s3://${BUCKET_TFSTATE} && \
			aws s3api put-bucket-versioning --bucket "${BUCKET_TFSTATE}" --versioning-configuration "Status=Enabled" && \
			echo '> Bucket created.'; \
		fi
check:
	${VALIDATE_CD} && aws s3 ls
fmt:
	${VALIDATE_CD} && terraform fmt
init: init_tfstate_s3
	${VALIDATE_CD} && terraform init
plan:
	${VALIDATE_CD} && terraform plan
apply:
	${VALIDATE_CD} && terraform apply
destroy:
	${VALIDATE_CD} && terraform destroy
output:
	${VALIDATE_CD} && terraform output ap_ids
# # mark tainted and delete next plan rebuild plan generated.
# taint:
# 	${VALIDATE_CD} && terraform taint
# draw:
# 	${VALIDATE_CD} && terraform graph -draw-cycles | dot -Tpng > graph.png
# state:
# 	terraform state show aws_vpc.tf_vpc
# import:
# 	terraform import aws_subnet.tf_subnet-private4 subnet-057a9cb8eee74a110
