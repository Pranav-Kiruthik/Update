// Response is an ARRAY
let tasks = pm.response.json();
let clientId = pm.environment.get("clientId");

if (!clientId) {
    throw new Error("clientId not found in environment");
}

// Filter tasks for client
let matchedTasks = tasks.filter(
    task => String(task["Client Id"]) === String(clientId)
);

console.log("Matched tasks:", matchedTasks);

if (matchedTasks.length === 0) {
    throw new Error("No tasks found for this clientId");
}

// Extract all caseIds
let caseIds = matchedTasks.map(task => task["Case Id"]);

// Save array + index
pm.environment.set("caseIds", JSON.stringify(caseIds));
pm.environment.set("caseIndex", 0);

console.log("Saved caseIds:", caseIds);

// Move to updateCase
postman.setNextRequest("updateCase");





let caseIds = JSON.parse(pm.environment.get("caseIds"));
let index = Number(pm.environment.get("caseIndex"));

if (index >= caseIds.length) {
    console.log("All cases updated");
    postman.setNextRequest(null); // stop execution
    return;
}

// Set current caseId
pm.environment.set("caseId", caseIds[index]);

console.log(`Updating case ${index + 1}/${caseIds.length}:`, caseIds[index]);
