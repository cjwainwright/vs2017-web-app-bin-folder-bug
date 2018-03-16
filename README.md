# Bug with Visual Studio adding dlls to web application bin folders that shouldn't be there as they are conditionally referenced

The solution contains a WebApplication project and two class library projects Reference1 and Reference2.
The WebApplication includes a reference to Reference1.
It also includes a reference to Reference2 but this is excluded by an msbuild Condition, as follows.

```xml
<ProjectReference Include="..\Reference1\Reference1.csproj">
  <Project>{ea9346c4-dd54-43ca-bc09-c16b2533e260}</Project>
  <Name>Reference1</Name>
</ProjectReference>
<ProjectReference Include="..\Reference2\Reference2.csproj" Condition="false">
  <Project>{3a8a0923-8437-4084-9ad9-04bf405fb464}</Project>
  <Name>Reference2</Name>
</ProjectReference>
```

The WebApplication bin folder should never contain Reference2.dll, however Visual Studio adds it when opening the solution.

## Repro steps

1. Clone the repository
2. Open the solution in Visual Studio 2017 
3. Note the folder WebApplication\bin is created, but empty as expected
4. Build the solution
5. Note the folder WebApplication\bin is populated with the correct dlls, namely Reference1.dll not Reference2.dll
6. Close the solution
7. Re-open the solution
8. Note the folder WebApplication\bin is now populated with additional incorrect dlls, namely Reference2.dll is added

