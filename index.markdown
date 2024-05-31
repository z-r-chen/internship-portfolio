---
layout: my_layout
---

<!-- # Internship Work Portfolio -->

## Introduction
During my internship at ITP Renewables, I made several contributions to various aspects of the project, including feature development, code refactoring, API optimization, and database interaction improvements. Below is a detailed report of my contributions.

## Techniques

Django, Django REST, React, Bootstrap, Google Cloud

## Contributions

<!-- ### Feature Development -->
- **Feature Development**: Developed features that provide users with additional input options, toggleable changelogs to key data fields, and new descriptive views. These enhancements improve user experience by offering more flexibility and transparency.

<!-- ### Code Refactoring -->
- **Code Refactoring**: Refactored React components from class-based to functional, introducing more generic abstractions. This transition leverages the advantages of React Hooks for easier life-cycle management, minimizes unnecessary re-renders, and improves code maintainability.

<!-- ### API Optimization -->
- **API Optimization**: Optimized API interactions by pruning redundant API calls on the frontend. Designed new model serializers and request handlers in Django REST to replace the previous nested serialization method, resulting in a more efficient data exchange pipeline.

<!-- ### Database Optimization -->
- **Database Optimization**: Optimized database interactions using Django ORM-related methods to prevent duplicate queries for fetching the same resource. Mitigated the N + 1 query problem existing in the codebase, enhancing overall database performance.

## Implementation

### Feature Development
1. **Additional Input Options**:
   - Implemented new input fields to enhance user input capabilities.
   - Ensured seamless integration with the existing data processing pipeline.

2. **Toggleable Changelogs**:
   - Developed a feature to allow users to toggle changelogs for key data fields.
![changelog](/images/changelog.png)
   - Enabled better tracking of changes and improved data management.

3. **Descriptive Views**:
   - Created new views that provide detailed descriptions of data.
   - Enhanced the user interface with informative and user-friendly displays.

### Code Refactoring
1. **React Component Refactoring**:
   - Transitioned from class-based to functional components.

```js
class function(a, b) {
    return x;
}
```

   - Utilized React Hooks (useState, useEffect, etc.) for state and life-cycle management.
   - Improved code readability and maintainability.

### API Optimization
1. **Redundant API Call Pruning**:
   - Identified and removed redundant API calls on the frontend.
   - Reduced unnecessary network requests, improving performance.

2. **Model Serializers and Request Handlers**:
   - Designed new model serializers in Django REST to streamline data serialization.
   - Created efficient request handlers to manage API requests.
   - Replaced nested serialization with a more efficient method, reducing payload size and processing time.

### Database Optimization
1. **Django ORM-related Methods**:
   - Utilized Django ORM-related methods (e.g., select_related, prefetch_related) to optimize query performance.
   - Addressed and mitigated the N + 1 query problem.
   - Ensured efficient resource fetching, reducing database load and improving response times.

## Conclusion
Throughout my internship, I focused on improving the project's overall efficiency, maintainability, and user experience. By developing new features, refactoring code, and optimizing both API and database interactions, I contributed to creating a more robust and performant system.