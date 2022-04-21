# verbose-bassoon
Testing differences in remediation advice and fix PRs when we chop the transitive line at points when we have already seen an artifact. 

<h3>1. First test with Snyk shows us:</h3>

<table>
  <tr>
      <th>With changes</th>
      <th>Maven-deps@4.12.0</th>
    </tr>
    <tr>
      <td>
        Only the first path through which the vulnerable transitive comes in is seen. There is no fix for this dependency.
<img wid<img width="1123" alt="no-remediqtion" src="https://user-images.githubusercontent.com/26426498/155332854-b6ee821e-5a3f-41f3-ae4b-c2c0b9b389b8.png">
    </td>
    <td>
      All paths on which this vulnerable transitive is are considered - the first one has no fix, but the others suggest bumping the version of the dependency. The fix PR modifies the version of poi and poi-ooxml
      <img width="1123" alt="1-commons-codec" src="https://user-images.githubusercontent.com/26426498/155532705-ffbb9aaa-41b7-4251-b3f8-89e87d116b8f.png">
      <img width="865" alt="Screenshot 2022-02-23 at 14 01 06" src="https://user-images.githubusercontent.com/26426498/155337176-8fa90824-387f-487a-80b3-8e115a8270ad.png">
    </td>
</tr>
</table>

<table>
  <tr>
     <th>With changes</th>
     <th>Maven-deps@4.12.0</th>
  </tr>
  <tr>
     <td>
<img width="648" alt="Screenshot 2022-02-23 at 15 10 44" src="https://user-images.githubusercontent.com/26426498/155335957-16059592-75c5-4173-8d4e-7ca75141c576.png">
    </td>
    <td>
<img width="806" alt="Screenshot 2022-02-23 at 15 10 57" src="https://user-images.githubusercontent.com/26426498/155336120-220dfc04-7cde-4a3f-9a60-3b9606fc9d3a.png">
    </td>
  </tr>
</table>

<table>
  <tr>
     <th>With changes</th>
     <th>Maven-deps@4.12.0</th>
  </tr>
  <tr>
     <td>
Snyk suggests fixing 3 vulnerabilities in poi (without commons-codec only seen in httpclient) - this can be done by ugrading to version 3.17
      <img width="486" alt="Screenshot 2022-02-24 at 14 30 34" src="https://user-images.githubusercontent.com/26426498/155533441-15d81d68-c5d8-4368-93df-414c2b3cefd8.png"> 
    </td>
    <td>
Snyk suggests fixing 4 vulnerabilities (including Information Exposure introduced through commons-codec) so we have to bump poi to version 4.1.1
      <img width="583" alt="Screenshot 2022-02-24 at 14 30 47" src="https://user-images.githubusercontent.com/26426498/155533696-b4871bbf-4326-44e9-9479-850f66f5184b.png">
    </td>
  </tr>
</table>

<h3>2. After merging the first fix PR we restest with Snyk and we see:</h3>


<table>
  <tr>
      <th>With changes</th>
      <th>Maven-deps@4.12.0</th>
    </tr>
    <tr>
      <td>
        Same as before because Snyk didn't offer a fix for this vulnerability.
<img width="900" alt="Screenshot 2022-02-23 at 15 16 49" src="https://user-images.githubusercontent.com/26426498/155337000-517e79e7-3b2c-4239-81de-7a1acd0f9bce.png">
    </td>
    <td>
      Despite bumping the version of poi and poi-ooxml we haven't fixed the issue because first seen (httpclient) is still at the vulnerable version. 
<img width="900" alt="dev_fix1_failed" src="https://user-images.githubusercontent.com/26426498/155336817-9ce34a09-fa25-4f8c-9f4a-2005cea2b913.png">
    </td>
</tr>
</table>

<table>
  <tr>
      <th>With changes</th>
      <th>Maven-deps@4.12.0</th>
    </tr>
    <tr>
      <td>
        The version of httpclient is changed but it didn't help with commons-codec
        <img width="890" alt="Screenshot 2022-02-23 at 15 27 25" src="https://user-images.githubusercontent.com/26426498/155339256-01f2a356-5292-4425-ba3a-c488cc363ec9.png">
    </td>
    <td>
      The version of poi and poi-ooxml have changed but it didn't help with the commons-codec issue because its version is still coming from httpclient
<img width="870" alt="Screenshot 2022-02-23 at 15 28 41" src="https://user-images.githubusercontent.com/26426498/155339421-c0ca6c86-f9c5-42d9-aec9-cd02799b78cd.png">
    </td>
</tr>
</table>

