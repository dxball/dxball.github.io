# Visual Studio 專案建置完成後取得組件版本號並輸出在文字檔中


接到一個需求，需要在專案編譯完成後，在執行檔同目錄下放一個文字檔 (version.txt)，內容為組件的版本號 (`AssemblyVersion`)

<!--more-->

## 需求

目前專案的版本號是直接定義在 AssemblyInfo.cs 中的 `AssemblyVersion`
無腦做法就是直接把 version.txt 加到專案裡面並設定為 Always output，每次要更新版號時，除了更新 AseesmblyInfo.cs，也要一併變更 version.txt 的內容

為了避免版本號分散在不同檔案中，所以就找了一個方法可以讓這個動作自動化

## 作法

1. 缷載目前專案，編輯 *.csproj
2. 加入自定建置後動作

    ```xml
      <Target Name="PostBuildMacros">
        <GetAssemblyIdentity AssemblyFiles="$(TargetPath)">
          <Output TaskParameter="Assemblies" ItemName="Targets" />
        </GetAssemblyIdentity>
        <ItemGroup>
          <VersionNumber Include="@(Targets->'%(Version)')" />
        </ItemGroup>
      </Target>
      <PropertyGroup>
        <PostBuildEventDependsOn>
          $(PostBuildEventDependsOn);
          PostBuildMacros;
        </PostBuildEventDependsOn>
        <PostBuildEvent>echo Version: @(VersionNumber) &gt; version.txt</PostBuildEvent>
      </PropertyGroup>
    ```

3. 重新載入專案，並進行建置，就會看到 version.txt 出現了

{{< admonition info >}}
可以在`專案屬性`->`建置事件`，編輯`建置後事件命令列`，修改為自行需要的動作
![Build Action](/2020/07/visual-studio-get-version-after-build/build_action.png)

{{< /admonition >}}

## Reference

- [Determine assembly version during a post-build event](https://stackoverflow.com/a/19371257/1568102)
- [GetAssemblyIdentity task](https://docs.microsoft.com/en-us/visualstudio/msbuild/getassemblyidentity-task)

