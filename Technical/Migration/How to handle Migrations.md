# Strategies
## Big bang
- Risky.
- Code freeze.
- All changes made together.
- Single deployment at the end.
## Incremental Approaches
- Agile approach.
- Less risk / disruption.
- No code freeze -> Continuous delivery of features while migrating.
- Time to value -> Value is consistently delivered as the migration happens.

# Phases of migration

![[Excalidraw/Strangler Fig.excalidraw.md#^clippedframe=TCAEZita|600]]
## Preparation:
- Review dependencies.
- Choose a target .NET
- Come up with a plan.
## Upgrade projects:
- Upgrade `csproj` format.
- NuGet `packages.config` to `PackageReference`
## Upgrade source:
- Update endpoints/features.
- Agile process.
## Test and deploy:
- Smoke test
- Regression test
- Deploy