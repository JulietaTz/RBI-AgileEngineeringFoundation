# Story 1.6 - T1T5 Test Cases

## Scope

User Story: `1-6-fallback-to-classic-discovery-without-session-loss`  
Goal: Validate fallback parity, no-trap behavior, session/cart continuity, and AI-optional checkout path.

## Preconditions

- Shopper is in guided discovery flow with fallback controls available.
- Guided flow has active journey context (`X-Journey-Id`) and non-empty cart state available for continuity checks.
- Classic discovery routes (search/categories/bestsellers) are reachable.
- Recommendation/session calls are routed through `RecommendationApiService`.

---

## T1 - Standard Case (Happy Path)

### T1_fallbackFromGuidedToSearchAndBack_validSession_userKeepsCartAndJourneyContext

**Requirement links:** AC1, AC2  
**Type:** Standard Case

**Steps**
1. Start in guided flow and add at least one product to cart.
2. Activate fallback via `Use search` (or equivalent search entry).
3. Browse/search and then return to guided flow.
4. Inspect cart contents and session/journey continuity signals.

**Expected Result**
- Fallback navigation works immediately with no forced AI completion step.
- Cart contents remain unchanged through mode switch.
- Journey context remains intact (no reset/new unrelated session).
- Guided flow can be resumed without dead-end or broken state.

---

## T2 - Alternative Case

### T2_fallbackViaCategoryOrBestsellers_fromGuided_userCanContinueClassicDiscoveryAndCheckoutWithoutAIGate

**Requirement links:** AC1, AC3, AC4  
**Type:** Alternative Case

**Steps**
1. Start in guided flow.
2. Switch using category browse or bestsellers (not search).
3. Continue shopping in classic flow and proceed to checkout.
4. Complete key checkout steps.

**Expected Result**
- Category/bestseller fallback behaves as first-class path.
- No mandatory AI gate appears before checkout.
- Shopper can continue and complete checkout using classic flow.
- Prior guided usage does not block or degrade classic purchase path.

---

## T3 - Exception Case

### T3_guidedRecommendationsUnavailable_userGetsFallbackOptionsAndAvoidsDeadEnd

**Requirement links:** AC4, AC1  
**Type:** Exception Case

**Steps**
1. Simulate weak/unavailable guided recommendation state (error/timeout/no useful set).
2. Observe presented recovery and fallback actions.
3. Choose fallback option and continue discovery.

**Expected Result**
- Fallback options are presented as valid peers, not hidden/degraded.
- User is not trapped in guided flow.
- Transition to classic discovery succeeds without losing core shopping state.
- UI remains usable and recoverable (no dead-end behavior).

---

## T4 - Negative Case

### T4_returnToGuidedAfterFallback_withInvalidOrMissingJourneyContext_systemReestablishesContinuityWithoutStateLoss

**Requirement links:** AC2  
**Type:** Negative Case

**Steps**
1. Enter guided flow and switch to classic discovery.
2. Simulate invalid/missing journey context on return to guided flow.
3. Re-enter guided flow and request recommendations.

**Expected Result**
- System handles missing/invalid context gracefully (re-establish or recover path).
- Cart state remains preserved.
- No crash, no blocked interaction, and no forced restart of shopping journey.
- Continuity semantics remain correct and observable.

---

## T5 - Misuse Case

### T5_rapidModeSwitchingBetweenGuidedAndClassic_userSessionAndCartRemainStableWithoutTrapOrCorruption

**Requirement links:** AC1, AC2, AC3  
**Type:** Misuse Case

**Steps**
1. Rapidly switch between guided flow and classic routes (search/categories/bestsellers) multiple times.
2. Add/remove items in cart during these switches.
3. Return to guided and then proceed toward checkout.

**Expected Result**
- No state corruption or broken navigation occurs under rapid switching.
- Cart remains consistent and usable.
- User never encounters mandatory AI gating before checkout.
- Session continuity remains stable and the journey is recoverable.

---

## Compact T1T5 Name List

- `T1_fallbackFromGuidedToSearchAndBack_validSession_userKeepsCartAndJourneyContext`
- `T2_fallbackViaCategoryOrBestsellers_fromGuided_userCanContinueClassicDiscoveryAndCheckoutWithoutAIGate`
- `T3_guidedRecommendationsUnavailable_userGetsFallbackOptionsAndAvoidsDeadEnd`
- `T4_returnToGuidedAfterFallback_withInvalidOrMissingJourneyContext_systemReestablishesContinuityWithoutStateLoss`
- `T5_rapidModeSwitchingBetweenGuidedAndClassic_userSessionAndCartRemainStableWithoutTrapOrCorruption`
