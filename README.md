const {
  Document, Packer, Paragraph, TextRun, HeadingLevel, AlignmentType,
  LevelFormat, BorderStyle, Table, TableRow, TableCell, WidthType, ShadingType
} = require('docx');
const fs = require('fs');

const border = { style: BorderStyle.SINGLE, size: 1, color: "CCCCCC" };
const borders = { top: border, bottom: border, left: border, right: border };

function h1(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_1,
    children: [new TextRun({ text, bold: true, size: 32, font: "Arial" })]
  });
}

function h2(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_2,
    children: [new TextRun({ text, bold: true, size: 26, font: "Arial" })]
  });
}

function h3(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_3,
    children: [new TextRun({ text, bold: true, size: 24, font: "Arial" })]
  });
}

function p(text, opts = {}) {
  return new Paragraph({
    spacing: { after: 120 },
    children: [new TextRun({ text, font: "Arial", size: 22, ...opts })]
  });
}

function code(text) {
  return new Paragraph({
    spacing: { after: 80, before: 80 },
    shading: { fill: "F3F3F3", type: ShadingType.CLEAR },
    border: { left: { style: BorderStyle.SINGLE, size: 6, color: "AAAAAA" } },
    indent: { left: 360 },
    children: [new TextRun({ text, font: "Courier New", size: 18, color: "333333" })]
  });
}

function bullet(text, opts = {}) {
  return new Paragraph({
    numbering: { reference: "bullets", level: 0 },
    spacing: { after: 80 },
    children: [new TextRun({ text, font: "Arial", size: 22, ...opts })]
  });
}

function errorBox(text) {
  return new Paragraph({
    spacing: { after: 120, before: 120 },
    shading: { fill: "FDE8E8", type: ShadingType.CLEAR },
    border: { left: { style: BorderStyle.SINGLE, size: 8, color: "CC3333" } },
    indent: { left: 360 },
    children: [new TextRun({ text: "ERROR: " + text, font: "Arial", size: 22, color: "CC3333" })]
  });
}

function fixBox(text) {
  return new Paragraph({
    spacing: { after: 120, before: 120 },
    shading: { fill: "E8F5E9", type: ShadingType.CLEAR },
    border: { left: { style: BorderStyle.SINGLE, size: 8, color: "2E7D32" } },
    indent: { left: 360 },
    children: [new TextRun({ text: "FIX: " + text, font: "Arial", size: 22, color: "2E7D32" })]
  });
}

function space() {
  return new Paragraph({ spacing: { after: 160 }, children: [new TextRun("")] });
}

function divider() {
  return new Paragraph({
    spacing: { after: 200, before: 200 },
    border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: "CCCCCC" } },
    children: [new TextRun("")]
  });
}

const doc = new Document({
  styles: {
    default: { document: { run: { font: "Arial", size: 22 } } },
    paragraphStyles: [
      { id: "Heading1", name: "Heading 1", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 32, bold: true, font: "Arial", color: "1A237E" },
        paragraph: { spacing: { before: 400, after: 200 }, outlineLevel: 0 } },
      { id: "Heading2", name: "Heading 2", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 26, bold: true, font: "Arial", color: "283593" },
        paragraph: { spacing: { before: 280, after: 160 }, outlineLevel: 1 } },
      { id: "Heading3", name: "Heading 3", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 24, bold: true, font: "Arial", color: "37474F" },
        paragraph: { spacing: { before: 200, after: 120 }, outlineLevel: 2 } },
    ]
  },
  numbering: {
    config: [
      { reference: "bullets",
        levels: [{ level: 0, format: LevelFormat.BULLET, text: "\u2022", alignment: AlignmentType.LEFT,
          style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] },
    ]
  },
  sections: [{
    properties: {
      page: {
        size: { width: 12240, height: 15840 },
        margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 }
      }
    },
    children: [

      // Title
      new Paragraph({
        alignment: AlignmentType.CENTER,
        spacing: { after: 120 },
        children: [new TextRun({ text: "Fintech Detection Lab", bold: true, size: 52, font: "Arial", color: "1A237E" })]
      }),
      new Paragraph({
        alignment: AlignmentType.CENTER,
        spacing: { after: 80 },
        children: [new TextRun({ text: "Week 1 Build Log — Complete Setup Documentation", size: 26, font: "Arial", color: "555555" })]
      }),
      new Paragraph({
        alignment: AlignmentType.CENTER,
        spacing: { after: 400 },
        children: [new TextRun({ text: "June 11, 2026", size: 22, font: "Arial", color: "888888" })]
      }),

      divider(),

      // Overview
      h1("Overview"),
      p("This document covers every step taken to set up the Fintech Detection Lab from scratch — including every error hit along the way, what caused it, and how it was fixed. Nothing is skipped."),
      p("The goal of this lab is to simulate real fintech attack scenarios (credential stuffing, insider data exfiltration, API abuse) and build detection rules that prove hands-on engineering capability — not just analyst review work."),
      space(),

      // Architecture
      h2("Lab Architecture"),
      p("Three VMs running on VirtualBox on a Windows 16GB host, all connected via a host-only network:"),
      space(),

      new Table({
        width: { size: 9360, type: WidthType.DXA },
        columnWidths: [2000, 2200, 1800, 3360],
        rows: [
          new TableRow({
            children: [
              new TableCell({ borders, shading: { fill: "1A237E", type: ShadingType.CLEAR },
                width: { size: 2000, type: WidthType.DXA },
                margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "VM", bold: true, font: "Arial", size: 20, color: "FFFFFF" })] })] }),
              new TableCell({ borders, shading: { fill: "1A237E", type: ShadingType.CLEAR },
                width: { size: 2200, type: WidthType.DXA },
                margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "IP", bold: true, font: "Arial", size: 20, color: "FFFFFF" })] })] }),
              new TableCell({ borders, shading: { fill: "1A237E", type: ShadingType.CLEAR },
                width: { size: 1800, type: WidthType.DXA },
                margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Specs", bold: true, font: "Arial", size: 20, color: "FFFFFF" })] })] }),
              new TableCell({ borders, shading: { fill: "1A237E", type: ShadingType.CLEAR },
                width: { size: 3360, type: WidthType.DXA },
                margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Role", bold: true, font: "Arial", size: 20, color: "FFFFFF" })] })] }),
            ]
          }),
          new TableRow({
            children: [
              new TableCell({ borders, width: { size: 2000, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Kali Linux", font: "Arial", size: 20 })] })] }),
              new TableCell({ borders, width: { size: 2200, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "192.168.56.10", font: "Courier New", size: 20 })] })] }),
              new TableCell({ borders, width: { size: 1800, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "2GB / 20GB", font: "Arial", size: 20 })] })] }),
              new TableCell({ borders, width: { size: 3360, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Attacker (Week 2 setup)", font: "Arial", size: 20 })] })] }),
            ]
          }),
          new TableRow({
            children: [
              new TableCell({ borders, shading: { fill: "F5F5F5", type: ShadingType.CLEAR }, width: { size: 2000, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Ubuntu 22.04", font: "Arial", size: 20 })] })] }),
              new TableCell({ borders, shading: { fill: "F5F5F5", type: ShadingType.CLEAR }, width: { size: 2200, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "192.168.56.20", font: "Courier New", size: 20 })] })] }),
              new TableCell({ borders, shading: { fill: "F5F5F5", type: ShadingType.CLEAR }, width: { size: 1800, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "2GB / 20GB", font: "Arial", size: 20 })] })] }),
              new TableCell({ borders, shading: { fill: "F5F5F5", type: ShadingType.CLEAR }, width: { size: 3360, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Victim / App server — Elastic Agent installed", font: "Arial", size: 20 })] })] }),
            ]
          }),
          new TableRow({
            children: [
              new TableCell({ borders, width: { size: 2000, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Ubuntu 22.04", font: "Arial", size: 20 })] })] }),
              new TableCell({ borders, width: { size: 2200, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "192.168.56.30", font: "Courier New", size: 20 })] })] }),
              new TableCell({ borders, width: { size: 1800, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "6GB / 50GB", font: "Arial", size: 20 })] })] }),
              new TableCell({ borders, width: { size: 3360, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Elastic SIEM — Elasticsearch + Kibana + Fleet Server", font: "Arial", size: 20 })] })] }),
            ]
          }),
        ]
      }),

      space(),
      divider(),

      // Step by step
      h1("Step-by-Step Build Log"),

      h2("Step 1 — VirtualBox Host-Only Network"),
      p("Before creating any VMs, a shared private network was configured in VirtualBox so the VMs could talk to each other without going through the internet."),
      bullet("File > Tools > Network Manager > Host-only Networks > Create"),
      bullet("IP set to 192.168.56.1, netmask 255.255.255.0"),
      bullet("DHCP disabled — IPs assigned manually per VM"),
      space(),

      h2("Step 2 — OS Selection"),
      p("Ubuntu Server 22.04 LTS was chosen for both VM2 (victim) and VM3 (SIEM). Ubuntu 26.04 was considered but ruled out — Elastic's apt repo does not have stable support for it yet, which would cause dependency errors mid-setup. Always use the version that matches Elastic's documented support matrix."),
      space(),

      h2("Step 3 — VM Creation"),
      p("Three VMs were created with the following config. Each VM was given two network adapters: Adapter 1 on NAT (for internet access during install), Adapter 2 on the host-only network (for inter-VM communication)."),
      bullet("Kali: 2GB RAM, 20GB disk, Debian 64-bit, IP 192.168.56.10"),
      bullet("Ubuntu victim: 2GB RAM, 20GB disk, Ubuntu 64-bit, IP 192.168.56.20"),
      bullet("Elastic SIEM: 4GB RAM initial (later increased), 50GB disk, Ubuntu 64-bit, IP 192.168.56.30"),
      space(),

      h2("Step 4 — Network Configuration on VM3"),
      p("After booting the elastic-siem VM, running ifconfig showed only one interface (enp0s3) with a NAT IP of 10.0.2.15. The host-only adapter was missing."),
      errorBox("Only one network interface showing — host-only adapter not attached"),
      fixBox("Shut down VM > VirtualBox Settings > Network > Adapter 2 > Enable > Host-only Adapter > vboxnet0 > OK > Start VM"),
      space(),
      p("After attaching Adapter 2, the netplan config was updated to assign the static IP:"),
      code("sudo nano /etc/netplan/00-installer-config.yaml"),
      code("network:"),
      code("  ethernets:"),
      code("    enp0s3:"),
      code("      dhcp4: true"),
      code("    enp0s8:"),
      code("      addresses: [192.168.56.30/24]"),
      code("      dhcp4: no"),
      code("  version: 2"),
      code("sudo netplan apply"),
      p("After applying, ifconfig confirmed enp0s8 showing 192.168.56.30. Network confirmed working."),
      space(),

      h2("Step 5 — Elastic Stack Installation"),
      p("Elasticsearch and Kibana were installed on VM3 using the official Elastic apt repo."),
      code("wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -"),
      code('echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list'),
      code("sudo apt update && sudo apt install -y elasticsearch kibana"),
      space(),

      h2("Step 6 — Kibana Not Accessible from Browser"),
      errorBox("http://192.168.56.30:5601 timing out — ERR_CONNECTION_TIMED_OUT"),
      p("Kibana was running but bound to localhost only. The kibana.yml config had server.host commented out, so it was not listening on the host-only interface."),
      fixBox("Edit /etc/kibana/kibana.yml — uncomment and set server.host to 192.168.56.30 and server.port to 5601"),
      code("server.port: 5601"),
      code('server.host: "192.168.56.30"'),
      code("sudo systemctl restart kibana"),
      space(),

      h2("Step 7 — Elasticsearch Crashing on Start (Exit Code 137)"),
      p("This was the most persistent issue. Exit code 137 means the process was killed by the OS due to out-of-memory. It happened repeatedly."),
      errorBox("Elasticsearch service failed — exit code 137 (OOM kill)"),
      p("First fix attempt: set heap size to 1GB in jvm.options.d. This did not work — the file was being written with the values but Elasticsearch still ran out of memory because 1GB was still too much given the initial 4GB VM allocation with the OS also consuming RAM."),
      fixBox("Two-part fix: reduce heap size to 512MB AND increase VM RAM from 4GB to 6GB"),
      code("sudo bash -c 'echo -e \"-Xms512m\\n-Xmx512m\" > /etc/elasticsearch/jvm.options.d/heap.options'"),
      p("VM was shut down, RAM increased to 6144MB in VirtualBox Settings > System > Motherboard, then restarted. Elasticsearch started successfully and stayed running."),
      space(),

      h2("Step 8 — Enrollment Token for Kibana"),
      p("Kibana requires an enrollment token to connect to Elasticsearch on first setup. The token is generated from the Elasticsearch side."),
      errorBox("Failed to determine the health of the cluster (exit code 69) — token generation failing because Elasticsearch was not running"),
      p("Once Elasticsearch was stable (after the RAM fix), the token was generated successfully:"),
      code("sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana"),
      p("The token was pasted into the Kibana browser setup page. A secondary 6-digit verification code was also required:"),
      code("sudo /usr/share/kibana/bin/kibana-verification-code"),
      space(),

      h2("Step 9 — Wrong Elastic Password"),
      errorBox("Username or password is incorrect — login failing on Kibana"),
      p("The elastic user password had been reset multiple times during troubleshooting. The most recently set password was no longer valid by the time login was attempted, because Elasticsearch had been restarted and the password reset was not the last one applied."),
      fixBox("Reset password fresh using the reset-password utility and use the output immediately"),
      code("sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic"),
      p("Final working password: lTFS=vuwopVo7RuLrIua"),
      space(),

      h2("Step 10 — SSH Setup for Copy-Paste"),
      p("All terminal work was being done directly in the VirtualBox window, which does not support copy-paste between the VM and the Windows host. This made working with long tokens and commands painful."),
      p("OpenSSH server was installed on the VM and SSH was enabled:"),
      code("sudo apt install -y openssh-server"),
      code("sudo systemctl enable ssh && sudo systemctl start ssh"),
      p("From that point on, all VM interaction was done via Windows Terminal over SSH, enabling full copy-paste."),
      space(),

      h2("Step 11 — Fleet Server Setup"),
      p("Fleet Server is required before enrolling Elastic Agents. It acts as the management layer between Kibana and the agents on monitored hosts."),
      p("In Kibana: Management > Fleet > Add Fleet Server. Fleet Server host was set to https://192.168.56.30:8220."),
      p("The Fleet Server install command from Kibana was run on the elastic-siem VM (not the victim VM)."),
      space(),
      errorBox("Fleet Server failing — dial tcp 10.0.2.x:9200 connection refused"),
      p("Fleet Server was trying to reach Elasticsearch on the NAT IP (10.0.2.x) instead of the host-only IP. This was because elasticsearch.yml did not have network.host explicitly set."),
      fixBox("Edit /etc/elasticsearch/elasticsearch.yml and set network.host to 192.168.56.30"),
      code("network.host: 192.168.56.30"),
      code("http.port: 9200"),
      code("transport.host: 192.168.56.30"),
      code("sudo systemctl restart elasticsearch"),
      space(),

      h2("Step 12 — Elastic Agent Installed on Wrong VM"),
      errorBox("Fleet Server install command accidentally run on ubuntu-victim instead of elastic-siem"),
      fixBox("Uninstall the agent from the victim VM, then re-run the correct Fleet Server command on elastic-siem"),
      code("sudo /opt/Elastic/Agent/elastic-agent uninstall"),
      space(),

      h2("Step 13 — Victim Agent Enrollment SSL Error"),
      p("After Fleet Server was confirmed healthy, the Elastic Agent install command was run on the victim VM to start shipping logs to the SIEM."),
      errorBox("x509: certificate signed by unknown authority — agent refusing to connect to Fleet Server"),
      p("Fleet Server uses a self-signed certificate by default. The agent needs to be told to skip certificate verification in a lab environment."),
      fixBox("Add --insecure flag to the elastic-agent install command"),
      code("sudo ./elastic-agent install \\"),
      code("  --url=https://192.168.56.30:8220 \\"),
      code("  --enrollment-token=<token> \\"),
      code("  --insecure"),
      space(),

      h2("Step 14 — Logs Confirmed in Kibana"),
      p("After successful agent enrollment, Kibana Discover was opened. Initial view showed no results due to a stale agent ID filter in the search bar and the wrong data view selected."),
      p("Fixed by clearing the filter and selecting the logs-* data view. Immediately showed 1,679 documents streaming from ubuntu-victim."),
      p("Week 1 complete."),
      space(),

      divider(),

      // Summary table
      h1("Error Summary"),
      p("All errors encountered and their resolutions, in order:"),
      space(),

      new Table({
        width: { size: 9360, type: WidthType.DXA },
        columnWidths: [300, 4200, 4860],
        rows: [
          new TableRow({
            children: [
              new TableCell({ borders, shading: { fill: "1A237E", type: ShadingType.CLEAR }, width: { size: 300, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "#", bold: true, font: "Arial", size: 20, color: "FFFFFF" })] })] }),
              new TableCell({ borders, shading: { fill: "1A237E", type: ShadingType.CLEAR }, width: { size: 4200, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Error", bold: true, font: "Arial", size: 20, color: "FFFFFF" })] })] }),
              new TableCell({ borders, shading: { fill: "1A237E", type: ShadingType.CLEAR }, width: { size: 4860, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                children: [new Paragraph({ children: [new TextRun({ text: "Fix", bold: true, font: "Arial", size: 20, color: "FFFFFF" })] })] }),
            ]
          }),
          ...[
            ["1", "Host-only adapter not showing in ifconfig", "Shut down VM, attach Adapter 2 as Host-only in VirtualBox settings"],
            ["2", "Kibana timing out — ERR_CONNECTION_TIMED_OUT", "Uncomment server.host in kibana.yml, set to 192.168.56.30"],
            ["3", "Elasticsearch exit code 137 (OOM kill)", "Set heap to 512m in jvm.options.d, increase VM RAM to 6GB"],
            ["4", "Enrollment token failing — cluster health error", "Elasticsearch was not running. Start it first, then generate token"],
            ["5", "Kibana enrollment token invalid", "OCR error reading token from screenshot. Always copy from terminal directly"],
            ["6", "Wrong elastic password", "Reset with elasticsearch-reset-password and use output immediately"],
            ["7", "Fleet Server connecting to NAT IP instead of host-only", "Set network.host in elasticsearch.yml to 192.168.56.30"],
            ["8", "Fleet Server install run on wrong VM", "Uninstall agent from victim, re-run Fleet Server command on SIEM VM"],
            ["9", "Agent SSL error — certificate signed by unknown authority", "Add --insecure flag to elastic-agent install command"],
            ["10", "No logs in Kibana Discover", "Clear stale agent ID filter, select logs-* data view"],
          ].map(([num, err, fix], i) =>
            new TableRow({
              children: [
                new TableCell({ borders, shading: { fill: i % 2 === 0 ? "FFFFFF" : "F5F5F5", type: ShadingType.CLEAR }, width: { size: 300, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                  children: [new Paragraph({ children: [new TextRun({ text: num, font: "Arial", size: 20 })] })] }),
                new TableCell({ borders, shading: { fill: i % 2 === 0 ? "FFFFFF" : "F5F5F5", type: ShadingType.CLEAR }, width: { size: 4200, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                  children: [new Paragraph({ children: [new TextRun({ text: err, font: "Arial", size: 20, color: "CC3333" })] })] }),
                new TableCell({ borders, shading: { fill: i % 2 === 0 ? "FFFFFF" : "F5F5F5", type: ShadingType.CLEAR }, width: { size: 4860, type: WidthType.DXA }, margins: { top: 80, bottom: 80, left: 120, right: 120 },
                  children: [new Paragraph({ children: [new TextRun({ text: fix, font: "Arial", size: 20, color: "2E7D32" })] })] }),
              ]
            })
          )
        ]
      }),

      space(),
      divider(),

      h1("Week 1 Completion Checklist"),
      bullet("3 VMs running and on the same host-only network", { bold: false }),
      bullet("Elasticsearch running on elastic-siem (192.168.56.30:9200)", { bold: false }),
      bullet("Kibana accessible from Windows browser (192.168.56.30:5601)", { bold: false }),
      bullet("Fleet Server enrolled and healthy", { bold: false }),
      bullet("Elastic Agent installed on ubuntu-victim (192.168.56.20)", { bold: false }),
      bullet("1,679 log documents confirmed flowing in Kibana Discover", { bold: false }),
      space(),

      divider(),

      h1("Next Steps — Week 2"),
      p("Week 2 covers the first attack scenario: credential stuffing against a mock fintech API."),
      bullet("Set up Kali VM (192.168.56.10) with host-only network"),
      bullet("Deploy a Flask-based mock fintech API on ubuntu-victim (port 5000)"),
      bullet("Run credential stuffing attack from Kali using Hydra or Python script"),
      bullet("Write first Sigma detection rule: high failed login rate from single IP"),
      bullet("Test evasion: slow requests, IP rotation via proxychains"),
      bullet("Document: what the rule catches, what it misses, how to harden it"),
      bullet("Commit Sigma rule + bypass writeup to GitHub repo"),
      space(),

    ]
  }]
});

Packer.toBuffer(doc).then(buffer => {
  fs.writeFileSync('/mnt/user-data/outputs/fintech_lab_week1_buildlog.docx', buffer);
  console.log('Done');
});
