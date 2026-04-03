<h1>🌐 Deploy a Website with S3 File Operations</h1>

<p>A fully automated AWS architecture to <b>upload</b>, <b>download</b>, and <b>delete</b> files from Amazon S3 using a Django web application.</p>

<hr/>

<h2>🧱 Architecture Overview</h2>

<hr/>

<h3>🔹 Networking</h3>
<ul>
  <li><b>Custom VPC:</b> 192.168.10.0/24 with DNS support</li>
  <li><b>Subnets:</b>
    <ul>
      <li><i>Public:</i> pub-a, pub-b → Internet-facing resources (Load Balancer, NAT Gateway)</li>
      <li><i>Private:</i> pvt-a, pvt-b → Backend EC2 instances</li>
    </ul>
  </li>
  <li><b>Routing:</b>
    <ul>
      <li><i>Public Route Table:</i> Connected to an <b>Internet Gateway</b> for public access</li>
      <li><i>Private Route Table:</i> Uses <b>NAT Gateway</b> for secure outbound traffic</li>
    </ul>
  </li>
</ul>

<hr/>

<h3>🔐 Security
