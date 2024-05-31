---
layout: my_layout
---

<!-- # Internship Work Portfolio -->

## Techniques

Django, Django REST, React, Bootstrap, Google Cloud

## Contributions Overview

<!-- ### Feature Development -->
- **Feature Development**: Developed features that provide users with additional input options, toggleable changelogs to key data fields, and new descriptive views.

<!-- ### Code Refactoring -->
- **Code Refactoring**: Refactored React components from class-based to functional, introducing more generic abstractions.

<!-- ### API Optimization -->
- **API Optimization**: Optimized API interactions by pruning redundant API calls on the frontend. Designed new model serializers and request handlers in Django REST to replace the previous nested serialization method.

<!-- ### Database Optimization -->
- **Database Optimization**: Optimized database interactions using Django ORM-related methods to prevent duplicate queries for fetching the same resource.

## Implementation

### Feature Development

I developed a feature shown below with the [`django-simple-history` library](https://django-simple-history.readthedocs.io/en/latest/) to allow users to toggle the change history for key data fields. This makes use of an additional `history` field to models that keeps track of historical changes made to the instance.

![changelog](/assets/images/changelog.png).

### Code Refactoring
I migrated our frontend components from class-based to functional with use of React Hooks (useState, useEffect, etc.) for state and life-cycle management, as recommended by [the official documentation](https://react.dev/reference/react/Component#alternatives).

Specifically, an example of `Project` class is demonstrated below. The class takes `projectData` as a prop, and maintains a state `approved` representing the approval status of the project. For the class-based component, it sets up the state in a constructor with the `super()` method. After that, it contains a `componentDidUpdate()` method which is invoked immediately after updating occurs. It is used to monitor some external changes that alter the approval status of the project.

As we can see from the code example, the functional implementation below utilizes two hooks `useState()` and `useEffect()` provided by React to achieve the same behavior as above. The first parameter of `useEffect()` is a callback function that runs when any member changes in the second parameter, which is a list of dependencies. It's more elegant in sense that we don't need to do conditional checking manually to examine if some prop has changed. Besides, when referring to the states, `this` is not necessary anymore because it's not a class but a function and thus we can get rid of the binding problems. A setter `setViewYear` is also returned when `useState()` invokes, so we don't nest bits in the dedicated `onChangeViewYear` function for this simple purpose.

As a consequence, this transition leverages the advantages of React Hooks for easier life-cycle management, minimises unnecessary re-renders, and improves code maintainability.

#### Class-based Component
```js
class Project extends Component {
    constructor(props) {
        super(props);
        this.state = {
            approved = projectData['approved'];
            viewYear = projectData['year'];
        };
    };

    componentDidUpdate(prevProps) {
        if (this.props.approved !== prevProps.approved) {
            // apply some logic on the change of project's approval status
        }
    }

    onChangeViewYear(year) {
        this.setState({
            viewYear: year
        });
    }

    render() {
        return ...; // JSX
    }
}
```
#### Functional Component
```js
function Project({ projectData }) {
    const approved = useRef(projectData['approved']);
    const [viewYear, setViewYear] = useState(projectData['year']);
    
    useEffect(() => {
        // apply some logic on the change of project's approval status
    }, [approved]);

    // no need for onChangeViewYear, use `setViewYear(year)` instead

    return ...; // JSX
}
```

### API Optimization

Assuming a class hierarchy as `Project - Milestone - Role - RoleMonthlyData`, when fetching or updating data through the API `/project/<project_id>`, the previous method is to reconstruct a JSON file like the example below representing the data structure in a project upon each request. To ensure an efficient data processing pipeline, I use a dedicated API endpoint with a corresponding serializer for each class, e.g., `/milestone/<milestone_id>`, `role_monthly_data/<role_monthly_data_id>`. Consequently, we can have a flattened JSON returned when establishing the project data, and we just need to construct the request body for the target instance when update. For example, we can update the instance `RoleMonthlyData(id=4, month="2024-02", hours=90, role_id=2)` through the API `role_monthly_data/4` using `POST` with the request body `{ "id": 4, "month": "2024-02", "hours": 120, "role_id": 2 }` instead of overwriting the entire data structure.

#### Nested Serialization

```json
{
  "id": 1,
  "name": "Project Alpha",
  "milestones": [
    {
      "id": 1,
      "name": "Milestone 1",
      "roles": [
        {
          "id": 1,
          "name": "Developer",
          "role_monthly_data": [
            {
              "id": 1,
              "month": "2024-01",
              "hours": 160
            },
            {
              "id": 2,
              "month": "2024-02",
              "hours": 150
            }
          ]
        },
        {
          "id": 2,
          "name": "Tester",
          "role_monthly_data": [
            {
              "id": 3,
              "month": "2024-01",
              "hours": 80
            },
            {
              "id": 4,
              "month": "2024-02",
              "hours": 90
            }
          ]
        }
      ]
    },
    {
      "id": 2,
      "name": "Milestone 2",
      "roles": [
        {
          "id": 3,
          "name": "Manager",
          "role_monthly_data": [
            {
              "id": 5,
              "month": "2024-03",
              "hours": 120
            }
          ]
        }
      ]
    }
  ]
}
```

#### New Method for Serialization

```json
{
  "project": {
    "id": 1,
    "name": "Project Alpha"
  },
  "milestones": [
    {
      "id": 1,
      "name": "Milestone 1",
      "project_id": 1
    },
    {
      "id": 2,
      "name": "Milestone 2",
      "project_id": 1
    }
  ],
  "roles": [
    {
      "id": 1,
      "name": "Developer",
      "milestone_id": 1
    },
    {
      "id": 2,
      "name": "Tester",
      "milestone_id": 1
    },
    {
      "id": 3,
      "name": "Manager",
      "milestone_id": 2
    }
  ],
  "role_monthly_data": [
    {
      "id": 1,
      "month": "2024-01",
      "hours": 160,
      "role_id": 1
    },
    {
      "id": 2,
      "month": "2024-02",
      "hours": 150,
      "role_id": 1
    },
    {
      "id": 3,
      "month": "2024-01",
      "hours": 80,
      "role_id": 2
    },
    {
      "id": 4,
      "month": "2024-02",
      "hours": 90,
      "role_id": 2
    },
    {
      "id": 5,
      "month": "2024-03",
      "hours": 120,
      "role_id": 3
    }
  ]
}
```

### Database Optimization
  
Considering the same class hierarchy as above, we can use the ORM method `prefetch_related()` in Django.

```py
projects = Project.objects.prefetch_related(
    'milestones',
    'milestones__roles',
    'milestones__roles__role_monthly_data',
)
```

This can be essentially translated to SQL statements as below.
```sql
SELECT * FROM Project;

SELECT * FROM Milestone WHERE project_id IN (SELECT id FROM Project);

SELECT * FROM Role WHERE milestone_id IN (
    SELECT id FROM Milestone WHERE project_id IN (SELECT id FROM Project)
);

SELECT * FROM RoleMonthlyData WHERE role_id IN (
    SELECT id FROM Role WHERE milestone_id IN (
        SELECT id FROM Milestone WHERE project_id IN (SELECT id FROM Project)
    )
);
```
With these four `SELECT` statements, instances of the four classes are fetched in a row. Otherwise, when we access a `RoleMonthlyData` instance via `project.milestone.role.role_monthly_data` in a nested manner, a `SELECT` statement is executed for each single read, causing the [N + 1 query problem](https://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem-in-orm-object-relational-mapping).

With prefetching, the backend query performance is optimized and the N + 1 query problem is addressed. This ensured efficient resource fetching, reducing database load and improving response times.

## Conclusion
Throughout my internship journey at ITP Renewables, I focused on improving the project's overall efficiency, maintainability, and user experience. By developing new features, refactoring code, and optimizing both API and database interactions, I contributed to creating a more robust and performant application.