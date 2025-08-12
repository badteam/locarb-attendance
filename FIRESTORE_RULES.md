# Firestore Rules (baseline)

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function signedIn() { return request.auth != null; }
    function isAdmin() {
      return signedIn() &&
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "admin";
    }

    match /users/{uid} {
      allow create: if signedIn() && request.auth.uid == uid;
      allow read: if signedIn() && (request.auth.uid == uid || isAdmin());
      allow update: if (signedIn() && request.auth.uid == uid &&
                        !request.resource.data.diff(resource.data)
                          .changedKeys().hasAny(["role","status","branchId","salary","allowances","deductions","showSalaryToUser"]))
                     || isAdmin();
    }

    match /users/{uid}/deviceTokens/{token} {
      allow create, read, delete: if request.auth != null && request.auth.uid == uid;
      allow update: if false;
    }

    match /companies/{companyId}/branches/{branchId} {
      allow read: if signedIn();
      allow write: if isAdmin();
    }

    match /assignments/{id} {
      allow read: if signedIn();
      allow create, update, delete: if isAdmin();
    }

    match /attendance/{uid}/days/{day} {
      allow read, create, update: if signedIn() && request.auth.uid == uid;
    }

    match /leaves/{id} {
      allow read: if signedIn();
      allow create: if signedIn() && request.auth.uid == request.resource.data.uid;
      allow update, delete: if isAdmin();
    }
  }
}
