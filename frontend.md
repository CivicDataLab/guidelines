# Frontend Development Guidelines

Welcome to our evolving set of guidelines for frontend development within our organization. These guidelines are aimed at promoting consistency, readability, and best practices throughout our projects. 
Below are the current recommendations specific to frontend development:

## Immer for Array and Object Manipulation
Utilize the Immer library whenever you need to work with arrays or objects. It enables immutable updates in a more intuitive manner, improving code maintainability.

## Avoid Array.reduce for Readability
While `array.reduce` is a powerful method, it can sometimes decrease code readability, leading to potential confusion for other developers. Consider using alternative approaches like map or forEach to make your code more straightforward and self-explanatory.

## Strict Equality (===)
Always use strict equality (===) for comparing variables. This ensures that both the value and data type are identical. Avoid using loose equality (==) to prevent unexpected behavior caused by type coercion.

## Error Boundaries for Robustness
Implement Error Boundaries strategically throughout your application. Create multiple error boundaries based on features or components to gracefully handle and display errors without causing application crashes.

## Sentry for Error Tracking
Integrate Sentry into your frontend projects to track and log errors effectively. Sentry provides valuable insights into production issues, enabling us to identify and resolve problems promptly.

## Accessibility Checker Tooling
Every frontend engineer must utilize accessibility checker tooling to ensure our applications are inclusive and usable by all. We recommend tools like the Wave extension to identify and address accessibility issues.


## Note
These guidelines are subject to updates and improvements as our development practices evolve. Always refer back to this document and stay informed about the latest changes to maintain code consistency and overall development efficiency.

Remember, adherence to these guidelines contributes to building high-quality, maintainable, and accessible frontend applications across our organization. Happy coding!
