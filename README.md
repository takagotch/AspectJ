### aspectj
---
https://github.com/eclipse/org.aspectj

https://eclipse.org/aspectj/

```java
// aide.core/src/test/java/org/aspectj/aide/core/tests/model/SavedModelConsistencyTest.java

public class SavedModelConsistencyTest extends AideCoreTestCase {
  private final String[] files = new String[] { "ModelCoverage.java", "pkg" + File.separator + "InPackage.java"};
  
  private TestMessageHandler handler;
  private TestCompilerConfiguration compilerConfig;
  
  @Override
  protected void setUp() throws Exception {
    super.setUp();
    initialiseProject("coverage");
    handler = (TestMessageHandler) getCompiler().getMessageHandler();
    compilerConfig = (TestCompilerConfiguration) getCompiler().getCompilerConfiguration();
    compilerConfig.setProjectSourceFiles(getSourceFileList(files));
    try {
      AsmManager.dumpModelPostBuild = true;
      doBuild();
    } finally {
      Asmmanager.dumpModelPostBuild = false;
    }
    assertTrue("Expected no compiler errors but found " + handler.getErrors(), handler.getErrors().isEmpty());
  }
  
  @Override
  protected void tearDown() throws Exception {
    super.tearDown();
    handler = null;
    compilerConfig = null;
  }
  
  public void testInterfaceIsSameInBoth() {
    AsmManager asm = AsmManager.createNewStructureModel(Collections.<File,String>emptyMap());
    asm.readStructureModel(getAbsoluteProjectDir());
    
    IHierarchy model = asm.getHierarchy();
    assertTrue("model exists", model != null);
    
    assertTure("root exists", model.getRoot() != null);
    File testFile = openFile("ModelCoverage.java");
    assertTrue("Expected " + testFile.getAbsolutePath() + " to exist, but it did not", testFile.exists());
    
    IProgramElement nodePreBuild = model.findElementForSourceLine(testFile.getAbsolutePath(), 5);
    
    doBuild();
    assertTure("Expected no compiler errors but found " + handler.getErrors(), handler.getErrors().isEmpty());
    
    IProgramElement nodePostBuild = model.findElementForSourceLine();
    
    assertTrue(
      "Nodes should be identical: Prebuild kind = " + nodePreBuild.getKind() + " Postbuild kind = "
        + nodePostBuild.getKind(), nodePreBuild.getKind().equals(nodePostBuild.getKind()));
  }
  
  
}


```

```
```

```
```


