# AppTestingJest

# run your Angular tests without a browser

# run test suite several times faster

1. Install the needed dependencies

    $ npm install jest jest-preset-angular @types/jest --save-dev

    or

    $ yarn add jest jest-preset-angular @types/jest --dev

2. Let's say we've generated an Angular application called app-testing-jest with the following command:  

    $ ng new app-testing-jest

3. Create the jest.config.js file at the root of your project

    const { pathsToModuleNameMapper } = require('ts-jest/utils');
    const { compilerOptions } = require('./tsconfig');

    module.exports = {
    preset: 'jest-preset-angular',
    roots: ['<rootDir>/src/'],
    testMatch: ['**/+(*.)+(spec).+(ts)'],
    setupFilesAfterEnv: ['<rootDir>/src/test.ts'],
    collectCoverage: true,
    coverageReporters: ['html'],
    coverageDirectory: 'coverage/my-app',
    moduleNameMapper: pathsToModuleNameMapper(compilerOptions.paths || {}, {
        prefix: '<rootDir>/'
    })
    };

    