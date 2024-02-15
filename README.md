# Cinema-Ticket-System
import java.sql.*;

class CinemaTicketSystem {
    static final String JDBC_URL = "jdbc:postgresql://localhost:5432/cts";
    static final String USERNAME = "Aisana";
    static final String PASSWORD = "qwerty1234";
    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD)) {
            Movie movie = new Movie("Harry Potter", "J. K. Rowling", 2001);
            createMovie(connection, movie);
            ResultSet resultSet = getAllMovies(connection);
            while (resultSet.next()) {
                String title = resultSet.getString("title");
                String director = resultSet.getString("director");
                int year = resultSet.getInt("year");
                System.out.println("Movie: " + title + ", Director: " + director + ", Year: " + year);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    static void createMovie(Connection connection, Movie movie) throws SQLException {
        String sql = "INSERT INTO movies (title, director, year) VALUES (?, ?, ?)";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, movie.getTitle());
            statement.setString(2, movie.getDirector());
            statement.setInt(3, movie.getYear());
            statement.executeUpdate();
        }
    }
    static ResultSet getAllMovies(Connection connection) throws SQLException {
        String sql = "SELECT * FROM movies";
        Statement statement = connection.createStatement();
        return statement.executeQuery(sql);
    }
}
class Movie {
    private String title;
    private String director;
    private int year;
    public Movie(String title, String director, int year) {
        this.title = title;
        this.director = director;
        this.year = year;
    }
    public String getTitle() {
        return title;
    }
    public String getDirector() {
        return director;
    }
    public int getYear() {
        return year;
    }
}static void createTicket(Connection connection, Ticket ticket) throws SQLException {
    String sql = "INSERT INTO tickets (ticket_number, user_email, movie_id) VALUES (?, ?, ?)";
    try (PreparedStatement statement = connection.prepareStatement(sql)) {
        statement.setInt(1, ticket.getTicketNumber());
        statement.setString(2, ticket.getUserEmail());
        statement.setInt(3, ticket.getMovieId());
        statement.executeUpdate();
    }
}
static ResultSet getAllTickets(Connection connection) throws SQLException {
    String sql = "SELECT * FROM tickets";
    Statement statement = connection.createStatement();
    return statement.executeQuery(sql);
}
class Ticket {
    private int ticketNumber;
    private String userEmail;
    private int movieId;
    public Ticket(int ticketNumber, String userEmail, int movieId) {
        this.ticketNumber = ticketNumber;
        this.userEmail = userEmail;
        this.movieId = movieId;
    }
    public int getTicketNumber() {
        return ticketNumber;
    }
    public String getUserEmail() {
        return userEmail;
    }
    public int getMovieId() {
        return movieId;
    }
}
