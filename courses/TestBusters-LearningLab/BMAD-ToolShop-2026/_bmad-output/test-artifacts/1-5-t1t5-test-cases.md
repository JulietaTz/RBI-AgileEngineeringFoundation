# Story 1.5 - T1T5 Test Cases

## Scope

User Story: `1-5-alternative-recommendations-and-refine-loop`  
Goal: Validate refine/alternative recommendation loop behavior, recovery triad, session continuity, and contract stability.

## Preconditions

- Shopper is on guided discovery page with `TaskPromptBar` available.
- Recommendation API session bootstrap works and returns a valid `journeyId`.
- Initial recommendation request already returned ranked cards (`items`, `meta`).
- Client uses `RecommendationApiService` for recommendation requests.

---

## T1 - Standard Case (Happy Path)

### T1_recommendationLoop_initialRankedSetWithRefine_userGetsNewRankedSetAndSessionIsPreserved

**Requirement links:** AC1, AC3, AC4  
**Type:** Standard Case

**Steps**
1. Submit a valid task + chips and wait for initial ranked set.
2. Click `Refine`.
3. Observe outgoing request payload.
4. Wait for response and rendered cards.

**Expected Result**
- Request is sent via `RecommendationApiService`.
- Request includes `mode=refine`.
- `X-Journey-Id` is included and unchanged from prior request.
- UI replaces previous cards with a new ranked set.
- Response contract remains `journeyId`, `items`, `meta`.

---

## T2 - Alternative Case

### T2_recommendationLoop_initialRankedSetWithMoreLikeThis_userGetsAlternativeRankedSetAroundSeedProduct

**Requirement links:** AC1, AC3, AC4  
**Type:** Alternative Case

**Steps**
1. Submit a valid task + chips and wait for initial ranked set.
2. On one recommendation card, click `More like this`.
3. Inspect outgoing request payload.
4. Wait for refreshed recommendation rendering.

**Expected Result**
- Request includes `mode=more_like_this`.
- Request includes `seedProductId` equal to clicked card product.
- `X-Journey-Id` is preserved.
- UI updates by replacing card list with alternative ranked set.
- Response keeps Story 1.4 contract shape.

---

## T3 - Exception Case

### T3_recommendationLoop_apiFailureAfterInitialSet_userSeesRecoveryTriadAndCanRecoverWithRetryOrRefineOrSearch

**Requirement links:** AC2  
**Type:** Exception Case

**Steps**
1. Render initial ranked set successfully.
2. Trigger refine or alternative action while API returns error (5xx/network error).
3. Inspect error-state actions in UI.
4. Click `Retry` and then `Refine` in separate executions.

**Expected Result**
- UI enters error state without dead end.
- Recovery actions are available in order: `Retry` -> `Refine` -> `Use search instead`.
- Retry/refine can trigger another request attempt.
- Search escape path remains usable.

---

## T4 - Negative Case

### T4_recommendationLoop_moreLikeThisWithoutValidSeedProductId_requestIsRejectedOrGracefullyHandledWithoutContractBreak

**Requirement links:** AC1, AC4  
**Type:** Negative Case

**Steps**
1. Trigger a `more_like_this` request with missing or invalid `seedProductId`.
2. Observe API behavior and UI handling.
3. Check whether stale or malformed data is rendered.

**Expected Result**
- System rejects invalid input (e.g., validation error) or applies documented graceful fallback.
- No crash and no broken UI state.
- Response/error structure remains stable and parseable.
- Existing Story 1.4 rendering behavior is not regressed.

---

## T5 - Misuse Case

### T5_recommendationLoop_repeatedRapidRefineAndAlternativeClicks_systemRemainsStableAndSessionConsistent

**Requirement links:** AC2, AC3, AC4  
**Type:** Misuse Case

**Steps**
1. From initial ranked set, rapidly alternate clicks between `Refine` and `More like this`.
2. Perform multiple requests in a short time window.
3. Observe session correlation, UI stability, and resulting list rendering.

**Expected Result**
- System remains responsive and does not enter broken/dead state.
- Requests continue to use `RecommendationApiService`.
- `X-Journey-Id` remains consistent throughout the loop.
- UI settles to a valid ranked set and keeps recovery/search actions accessible.
- Contract shape remains stable despite interaction abuse.

---

## Compact T1T5 Name List

- `T1_recommendationLoop_initialRankedSetWithRefine_userGetsNewRankedSetAndSessionIsPreserved`
- `T2_recommendationLoop_initialRankedSetWithMoreLikeThis_userGetsAlternativeRankedSetAroundSeedProduct`
- `T3_recommendationLoop_apiFailureAfterInitialSet_userSeesRecoveryTriadAndCanRecoverWithRetryOrRefineOrSearch`
- `T4_recommendationLoop_moreLikeThisWithoutValidSeedProductId_requestIsRejectedOrGracefullyHandledWithoutContractBreak`
- `T5_recommendationLoop_repeatedRapidRefineAndAlternativeClicks_systemRemainsStableAndSessionConsistent`
