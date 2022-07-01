# -*- mode: Python -*-

k8s_yaml([
	"dev-tools/dev-bucket.yaml",
])
k8s_resource(
	"dev-bucket",
	port_forwards=9000
)

local_resource(
	"mc-alias",
	"mc alias set dev http://localhost:9000 minio minio123",
	resource_deps=["dev-bucket"],
	labels=["mc"]
)

local_resource(
	"mc-dev-bucket",
	"mc mb --ignore-existing dev/dev-bucket",
	resource_deps=["mc-alias"],
	labels=["mc-dev-bucket"]
)

k8s_yaml([
	"dev-tools/dev.yaml",
])

local_resource(
	"sync",
	"mc mirror --overwrite --remove ./dev-bucket dev/dev-bucket; flux -n app reconcile ks dev-ks --with-source",
	labels=["sync"]
)
