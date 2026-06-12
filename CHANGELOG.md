# Changelog

## 1.1.3
- `fix-deploy` skill and agent now classify transient recreate-rollout noise (the previous pod transiently `Failed` while the new pod is still `Pending`) and steer toward a safe retry instead of app changes
- `setup-cicd` skill and agent now carry a reusable-actions reference (`build-oci-image`, `upload-image-to-pluglayer`, `redeploy-pluglayer-app`, `apply-env-and-restart`), forbid hand-rolled polling/retry loops, and require a per-app `concurrency` group; the redeploy and env-apply actions wait for their task and surface real rollout failure detail

## 1.0.0
- Initial Cursor plugin package aligned with Cursor's plugin template shape
- Added PlugLayer MCP wiring, deployment rule, and deployment/domain/debugging skills
