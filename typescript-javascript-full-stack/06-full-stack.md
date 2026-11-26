# Full-stack architecture

## Browser security
CORS is a browser policy, not authentication. Use Secure and HttpOnly session cookies with appropriate SameSite settings. Protect cookie-based mutations from CSRF; prevent XSS with safe rendering. Never embed secrets in frontend bundles.

## Rendering and messaging
CSR renders in the browser; SSR sends initial HTML and hydration attaches interactivity. SSE suits server-to-client token streaming; WebSocket supports bidirectional communication. Handle cancellation, reconnection, authorization and flow control.

## AI chat design exercise
Authenticate users, validate prompts, enforce concurrency and token budgets, call the model and stream results. Persist conversation IDs and final messages. Abort abandoned requests, monitor latency and cost, and validate tool arguments and permissions server-side.

## Testing
Unit-test pure logic; integration-test real database constraints and APIs; use browser end-to-end tests for workflows. Test races with controlled promises, not sleeps. Review AI-generated code for invariants, failure modes, security and concurrency; run type checking, linting and CI.
