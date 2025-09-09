import java.time.*;
import java.util.*;
import java.util.stream.Collectors;

/* =========================
 * Domain: Users & Roles
 * ========================= */
class User {
    private static int NEXT_ID = 1;

    private final int id;             // encapsulated fields
    private String name;
    private String role;
    private String email;
    private String phone;

    public User(String name, String role, String email, String phone) {
        this.id = NEXT_ID++;
        this.name = name;
        this.role = role;
        this.email = email;
        this.phone = phone;
    }

    // Getters/Setters (Encapsulation)
    public int getId()               { return id; }
    public String getName()          { return name; }
    public String getRole()          { return role; }
    public String getEmail()         { return email; }
    public String getPhone()         { return phone; }

    public void setName(String name)       { this.name = name; }
    public void setRole(String role)       { this.role = role; }
    public void setEmail(String email)     { this.email = email; }
    public void setPhone(String phone)     { this.phone = phone; }

    // Helper methods
    public boolean canApproveConversions() { return false; } // Overridden in SalesManager

    // Polymorphic display (Overriding)
    @Override
    public String toString() {
        return "User{id=%d, name='%s', role='%s', email='%s', phone='%s'}"
                .formatted(id, name, role, email, phone);
    }
}

// Inheritance + Polymorphism: SalesManager extends User
class SalesManager extends User {
    private boolean approvalPrivilege; // extra capability
    private int approvalLimitScore;    // leads with score < limit need manager approval

    public SalesManager(String name, String email, String phone, int approvalLimitScore) {
        super(name, "SalesManager", email, phone);
        this.approvalPrivilege = true;
        this.approvalLimitScore = approvalLimitScore;
    }

    public boolean hasApprovalPrivilege() { return approvalPrivilege; }
    public int getApprovalLimitScore()    { return approvalLimitScore; }
    public void setApprovalLimitScore(int score) { this.approvalLimitScore = score; }

    @Override
    public boolean canApproveConversions() { return approvalPrivilege; }

    @Override
    public String toString() {
        return "SalesManager{%s, approvalLimitScore=%d}"
                .formatted(super.toString(), approvalLimitScore);
    }
}

/* =========================
 * Domain: Leads & Customers
 * ========================= */
enum LeadStatus { NEW, CONTACTED, QUALIFIED, CONVERTED, LOST }

class Lead {
    private static int NEXT_LEAD_ID = 1000;

    private final int leadId;
    private String company;
    private String contactPerson;
    private LeadStatus status;
    private int score;                // 0–100
    private String email;
    private String phone;
    private Integer ownerUserId;      // assigned CRM user
    private LocalDate createdOn;
    private LocalDate convertedOn;

    public Lead(String company, String contactPerson, String email, String phone, int score, Integer ownerUserId) {
        this.leadId = NEXT_LEAD_ID++;
        this.company = company;
        this.contactPerson = contactPerson;
        this.email = email;
        this.phone = phone;
        this.status = LeadStatus.NEW;
        this.score = score;
        this.ownerUserId = ownerUserId;
        this.createdOn = LocalDate.now();
        this.convertedOn = null;
    }

    // Getters/Setters
    public int getLeadId()                  { return leadId; }
    public String getCompany()              { return company; }
    public String getContactPerson()        { return contactPerson; }
    public String getEmail()                { return email; }
    public String getPhone()                { return phone; }
    public LeadStatus getStatus()           { return status; }
    public int getScore()                   { return score; }
    public Integer getOwnerUserId()         { return ownerUserId; }
    public LocalDate getCreatedOn()         { return createdOn; }
    public LocalDate getConvertedOn()       { return convertedOn; }

    public void setCompany(String company)              { this.company = company; }
    public void setContactPerson(String contactPerson)  { this.contactPerson = contactPerson; }
    public void setEmail(String email)                  { this.email = email; }
    public void setPhone(String phone)                  { this.phone = phone; }
    public void setStatus(LeadStatus status)            { this.status = status; }
    public void setScore(int score)                     { this.score = score; }
    public void setOwnerUserId(Integer ownerUserId)     { this.ownerUserId = ownerUserId; }
    public void markConverted() {
        this.status = LeadStatus.CONVERTED;
        this.convertedOn = LocalDate.now();
    }

    // Overriding display
    @Override
    public String toString() {
        return "Lead{id=%d, company='%s', contact='%s', status=%s, score=%d, owner=%s, phone=%s, email=%s}"
                .formatted(leadId, company, contactPerson, status, score,
                        ownerUserId == null ? "-" : ownerUserId, phone, email);
    }
}

class Customer {
    private static int NEXT_CUSTOMER_ID = 5000;

    private final int customerId;
    private String company;
    private String primaryContact;
    private String phone;
    private String email;
    private List<String> contracts;      // contracts or subscriptions
    private LocalDate createdOn;
    private Integer accountOwnerUserId;  // CSM / AM

    public Customer(String company, String primaryContact, String phone, String email, Integer ownerId) {
        this.customerId = NEXT_CUSTOMER_ID++;
        this.company = company;
        this.primaryContact = primaryContact;
        this.phone = phone;
        this.email = email;
        this.contracts = new ArrayList<>();
        this.createdOn = LocalDate.now();
        this.accountOwnerUserId = ownerId;
    }

    // Getters/Setters
    public int getCustomerId()                 { return customerId; }
    public String getCompany()                 { return company; }
    public String getPrimaryContact()          { return primaryContact; }
    public String getPhone()                   { return phone; }
    public String getEmail()                   { return email; }
    public List<String> getContracts()         { return Collections.unmodifiableList(contracts); }
    public LocalDate getCreatedOn()            { return createdOn; }
    public Integer getAccountOwnerUserId()     { return accountOwnerUserId; }

    public void setCompany(String company)                  { this.company = company; }
    public void setPrimaryContact(String primaryContact)    { this.primaryContact = primaryContact; }
    public void setPhone(String phone)                      { this.phone = phone; }
    public void setEmail(String email)                      { this.email = email; }
    public void setAccountOwnerUserId(Integer owner)        { this.accountOwnerUserId = owner; }

    // Behavior methods
    public void addContract(String contractName) { contracts.add(contractName); }
    public boolean hasContracts()                { return !contracts.isEmpty(); }

    // Overriding display
    @Override
    public String toString() {
        return "Customer{id=%d, company='%s', primaryContact='%s', owner=%s, contracts=%s}"
                .formatted(customerId, company, primaryContact,
                        accountOwnerUserId == null ? "-" : accountOwnerUserId, contracts);
    }
}

/* =========================
 * Service Layer: CRMManager
 * ========================= */
class CRMManager {
    // Storage
    private final Map<Integer, User> users = new HashMap<>();
    private final Map<Integer, Lead> leads = new HashMap<>();
    private final Map<Integer, Customer> customers = new HashMap<>();

    // Settings
    private int autoConvertScoreThreshold = 70; // below this needs SalesManager approval

    // CRUD / Adders
    public User addUser(User user) {
        users.put(user.getId(), user);
        return user;
    }
    public Lead addLead(Lead lead) {
        leads.put(lead.getLeadId(), lead);
        return lead;
    }
    public Customer addCustomer(Customer customer) {
        customers.put(customer.getCustomerId(), customer);
        return customer;
    }

    // Getters
    public Collection<User> listUsers()         { return users.values(); }
    public Collection<Lead> listLeads()         { return leads.values(); }
    public Collection<Customer> listCustomers() { return customers.values(); }

    // --- Method Overloading: search() by different params ---
    public List<Lead> search(String nameOrContactPerson) {
        String q = nameOrContactPerson.toLowerCase();
        return leads.values().stream()
                .filter(l -> l.getContactPerson().toLowerCase().contains(q))
                .sorted(Comparator.comparing(Lead::getLeadId))
                .collect(Collectors.toList());
    }

    // Same method name, extra parameter toggles company-only search (overload)
    public List<Lead> search(String company, boolean companyOnly) {
        String q = company.toLowerCase();
        return leads.values().stream()
                .filter(l -> l.getCompany().toLowerCase().contains(q))
                .sorted(Comparator.comparing(Lead::getLeadId))
                .collect(Collectors.toList());
    }

    public List<Lead> search(long phoneAsNumber) {
        String q = String.valueOf(phoneAsNumber);
        return leads.values().stream()
                .filter(l -> l.getPhone() != null && l.getPhone().contains(q))
                .sorted(Comparator.comparing(Lead::getLeadId))
                .collect(Collectors.toList());
    }

    // Filters
    public List<Lead> listByStatus(LeadStatus status) {
        return leads.values().stream()
                .filter(l -> l.getStatus() == status)
                .sorted(Comparator.comparing(Lead::getLeadId))
                .collect(Collectors.toList());
    }

    public List<Lead> listByMinScore(int minScore) {
        return leads.values().stream()
                .filter(l -> l.getScore() >= minScore)
                .sorted(Comparator.comparing(Lead::getScore).reversed())
                .collect(Collectors.toList());
    }

    // Conversion with approval logic (Polymorphism via User reference)
    public Optional<Customer> convertLead(User approver, int leadId) {
        Lead lead = leads.get(leadId);
        if (lead == null) {
            System.out.println("Lead " + leadId + " not found.");
            return Optional.empty();
        }
        if (lead.getStatus() == LeadStatus.CONVERTED) {
            System.out.println("Lead " + leadId + " is already converted.");
            return Optional.empty();
        }

        boolean requiresApproval = lead.getScore() < autoConvertScoreThreshold;
        if (requiresApproval && (approver == null || !approver.canApproveConversions())) {
            System.out.println("Lead " + leadId + " requires SalesManager approval (score="
                    + lead.getScore() + ", threshold=" + autoConvertScoreThreshold + ").");
            return Optional.empty();
        }

        // Perform conversion
        Customer c = new Customer(
                lead.getCompany(),
                lead.getContactPerson(),
                lead.getPhone(),
                lead.getEmail(),
                lead.getOwnerUserId()
        );
        addCustomer(c);
        lead.markConverted();
        System.out.println("Converted Lead " + leadId + " -> Customer " + c.getCustomerId());
        return Optional.of(c);
    }

    // Reports
    public Map<Integer, Long> reportLeadsPerOwner() {
        return leads.values().stream()
                .collect(Collectors.groupingBy(
                        l -> l.getOwnerUserId() == null ? -1 : l.getOwnerUserId(),
                        Collectors.counting()
                ));
    }

    public long reportConversionsInLastDays(int days) {
        LocalDate cutoff = LocalDate.now().minusDays(days);
        return leads.values().stream()
                .filter(l -> l.getStatus() == LeadStatus.CONVERTED)
                .filter(l -> l.getConvertedOn() != null && !l.getConvertedOn().isBefore(cutoff))
                .count();
    }

    public void setAutoConvertScoreThreshold(int threshold) {
        this.autoConvertScoreThreshold = threshold;
    }
}

/* =========================
 * Console App (Main)
 * ========================= */
public class CRMAppMain {
    private static final Scanner SC = new Scanner(System.in);
    private static final CRMManager CRM = new CRMManager();

    private static void seedSampleData() {
        // Users (Polymorphism demo: SalesManager via User reference)
        User u1 = CRM.addUser(new User("Asha Verma", "SalesRep", "asha@crm.com", "9001002003"));
        User u2 = CRM.addUser(new User("Ravi Nair", "SalesRep", "ravi@crm.com", "9001002004"));
        User manager = CRM.addUser(new SalesManager("Priya Iyer", "priya@crm.com", "9001002005", 70)); // User ref to SalesManager

        // Leads
        CRM.addLead(new Lead("Acme Corp", "John Stone", "john@acme.com", "9988776655", 82, u1.getId()));
        CRM.addLead(new Lead("ZenSoft", "Meera Rao", "meera@zensoft.io", "7788996655", 65, u1.getId()));
        CRM.addLead(new Lead("NovaTech", "Karan Shah", "karan@novatech.ai", "8899776611", 55, u2.getId()));
        CRM.addLead(new Lead("Acme Corp", "Lara D'Souza", "lara@acme.com", "9988001100", 92, u2.getId()));

        // Convert one high-score lead without needing approval
        CRM.convertLead(u1, 1000); // score 82 >= 70 threshold -> allowed without manager
    }

    private static void menu() {
        System.out.println("\n===== CRM Console =====");
        System.out.println("1. List Users");
        System.out.println("2. Add User");
        System.out.println("3. List Leads");
        System.out.println("4. Add Lead");
        System.out.println("5. Convert Lead to Customer");
        System.out.println("6. Search Leads (by name/company/phone)");
        System.out.println("7. Filter Leads by Status");
        System.out.println("8. Filter Leads by Min Score");
        System.out.println("9. List Customers");
        System.out.println("10. Reports");
        System.out.println("0. Exit");
        System.out.print("Choose: ");
    }

    private static int readInt() {
        while (true) {
            String s = SC.nextLine().trim();
            try { return Integer.parseInt(s); }
            catch (Exception e) { System.out.print("Enter number: "); }
        }
    }

    private static long readLong() {
        while (true) {
            String s = SC.nextLine().trim();
            try { return Long.parseLong(s); }
            catch (Exception e) { System.out.print("Enter number: "); }
        }
    }

    private static void listUsers() {
        CRM.listUsers().forEach(System.out::println);
    }

    private static void addUser() {
        System.out.print("Name: "); String name = SC.nextLine();
        System.out.print("Role (SalesRep/Support/etc): "); String role = SC.nextLine();
        System.out.print("Email: "); String email = SC.nextLine();
        System.out.print("Phone: "); String phone = SC.nextLine();

        User u;
        if ("SalesManager".equalsIgnoreCase(role)) {
            System.out.print("Approval limit score (leads below this need approval): ");
            int limit = readInt();
            u = new SalesManager(name, email, phone, limit);
        } else {
            u = new User(name, role, email, phone);
        }
        CRM.addUser(u);
        System.out.println("Added: " + u);
    }

    private static void listLeads() {
        CRM.listLeads().forEach(System.out::println);
    }

    private static void addLead() {
        System.out.print("Company: "); String company = SC.nextLine();
        System.out.print("Contact Person: "); String contact = SC.nextLine();
        System.out.print("Email: "); String email = SC.nextLine();
        System.out.print("Phone: "); String phone = SC.nextLine();
        System.out.print("Score (0-100): "); int score = readInt();
        System.out.print("Owner User ID (or -1 for none): "); int owner = readInt();
        Integer ownerId = owner < 0 ? null : owner;

        Lead lead = new Lead(company, contact, email, phone, score, ownerId);
        CRM.addLead(lead);
        System.out.println("Added: " + lead);
    }

    private static void convertLead() {
        System.out.print("Lead ID to convert: "); int leadId = readInt();
        System.out.print("Approver User ID (SalesManager or any user): "); int approverId = readInt();
        User approver = CRM.listUsers().stream().filter(u -> u.getId() == approverId).findFirst().orElse(null);
        if (approver == null) {
            System.out.println("Approver not found.");
            return;
        }
        CRM.convertLead(approver, leadId);
    }

    private static void searchLeads() {
        System.out.println("Search by: 1) Name  2) Company  3) Phone");
        int t = readInt();
        switch (t) {
            case 1 -> {
                System.out.print("Enter name (contact person) contains: ");
                String q = SC.nextLine();
                CRM.search(q).forEach(System.out::println);
            }
            case 2 -> {
                System.out.print("Enter company contains: ");
                String q = SC.nextLine();
                CRM.search(q, true).forEach(System.out::println);
            }
            case 3 -> {
                System.out.print("Enter phone (digits): ");
                long p = readLong();
                CRM.search(p).forEach(System.out::println);
            }
            default -> System.out.println("Invalid choice.");
        }
    }

    private static void filterByStatus() {
        System.out.println("Status: NEW, CONTACTED, QUALIFIED, CONVERTED, LOST");
        System.out.print("Enter status: ");
        try {
            LeadStatus st = LeadStatus.valueOf(SC.nextLine().trim().toUpperCase());
            CRM.listByStatus(st).forEach(System.out::println);
        } catch (IllegalArgumentException ex) {
            System.out.println("Invalid status.");
        }
    }

    private static void filterByScore() {
        System.out.print("Min score: ");
        int s = readInt();
        CRM.listByMinScore(s).forEach(System.out::println);
    }

    private static void listCustomers() {
        CRM.listCustomers().forEach(System.out::println);
    }

    private static void reports() {
        System.out.println("=== Reports ===");
        System.out.println("1) Leads per Owner");
        System.out.println("2) Conversions in last 7 days");
        System.out.println("3) Set Auto-Convert Score Threshold");
        System.out.print("Choose: ");
        int c = readInt();
        switch (c) {
            case 1 -> {
                Map<Integer, Long> map = CRM.reportLeadsPerOwner();
                System.out.println("OwnerUserId -> Count (OwnerUserId -1 means unassigned)");
                map.forEach((k, v) -> System.out.println(k + " -> " + v));
            }
            case 2 -> {
                long count = CRM.reportConversionsInLastDays(7);
                System.out.println("Conversions in last 7 days: " + count);
            }
            case 3 -> {
                System.out.print("Enter new auto-convert threshold (0-100): ");
                int th = readInt();
                CRM.setAutoConvertScoreThreshold(th);
                System.out.println("Updated threshold.");
            }
            default -> System.out.println("Invalid choice.");
        }
    }

    public static void main(String[] args) {
        seedSampleData();

        while (true) {
            menu();
            int choice = readInt();
            switch (choice) {
                case 1 -> listUsers();
                case 2 -> addUser();
                case 3 -> listLeads();
                case 4 -> addLead();
                case 5 -> convertLead();
                case 6 -> searchLeads();
                case 7 -> filterByStatus();
                case 8 -> filterByScore();
                case 9 -> listCustomers();
                case 10 -> reports();
                case 0 -> { System.out.println("Goodbye!"); return; }
                default -> System.out.println("Invalid option.");
            }
        }
    }
}
