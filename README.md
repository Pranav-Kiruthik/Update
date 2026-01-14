let caseIds = JSON.parse(pm.environment.get("caseIds"));
let caseIndex = Number(pm.environment.get("caseIndex"));

caseIndex++;
pm.environment.set("caseIndex", caseIndex);

if (caseIndex < caseIds.length) {
    postman.setNextRequest("updateCase");
} else {
    postman.setNextRequest(null);
}