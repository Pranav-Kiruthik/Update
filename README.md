let res = pm.response.json();
let clientId = pm.environment.get("clientId");

if (!res.tasks || !Array.isArray(res.tasks)) {
    throw new Error("No tasks array found");
}

// Filter by clientId
let matchedTasks = res.tasks.filter(task =>
    String(task["Client Id"]) === String(clientId)
);

console.log("clientId from env:", clientId);
console.log("matched tasks:", matchedTasks);

if (matchedTasks.length === 0) {
    throw new Error("No tasks found for this clientId");
}

// Collect ALL caseIds
let caseIds = matchedTasks.map(task => task["Case Id"]);

// Save for looping
pm.environment.set("caseIds", JSON.stringify(caseIds));
pm.environment.set("caseIndex", 0);

console.log("Saved caseIds:", caseIds);

// Move to updateCase
postman.setNextRequest("updateCase");







let caseIds = JSON.parse(pm.environment.get("caseIds"));
let index = Number(pm.environment.get("caseIndex"));

if (index >= caseIds.length) {
    console.log("All cases updated");
    postman.setNextRequest(null);
    return;
}

// Set current caseId
pm.environment.set("caseId", caseIds[index]);
console.log("Updating caseId:", caseIds[index]);







let index = Number(pm.environment.get("caseIndex"));
pm.environment.set("caseIndex", index + 1);

// Call updateCase again
postman.setNextRequest("updateCase");





