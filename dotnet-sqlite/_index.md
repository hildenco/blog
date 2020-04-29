# dotnet - Sqlite
Todo : test, GH, blog post


// connect + select

using System;
using Microsoft.Data.Sqlite;

namespace SqliteConsoleTest
{
  public class Program
  {
    public static void Main(string[] args)
    {
      using (var connection = new SqliteConnection(
        "Data Source=:memory:"))        
      {
        connection.Open();
        var command = new SqliteCommand(            
          "SELECT 1;", connection);                      
        long result = (long)command.ExecuteScalar();      
        Console.WriteLine($"Command output: {result}");
      }                                                 
    }
  }
}


// create table 

using System;
using System.Data.Common;                                       1
using Microsoft.Data.Sqlite;

namespace SqliteScmTest
{
  public class SampleScmDataFixture : IDisposable               2
  {
    public SqliteConnection Connection { get; private set; }

    public SampleScmDataFixture()
    {
      var conn = new SqliteConnection("Data Source=:memory:");
      Connection = conn;
      conn.Open();

      var command = new SqliteCommand(
        @"CREATE TABLE PartType(
            Id INTEGER PRIMARY KEY,
            Name VARCHAR(255) NOT NULL                          5
          );", conn);
      command.ExecuteNonQuery();                                6
      command = new SqliteCommand(
        @"INSERT INTO PartType                                  7
            (Name)                                              8.           VALUES

            ('8289 L-shaped plate')",                           9
        conn);
      command.ExecuteNonQuery();
    }

    public void Dispose()
    {
      if (Connection != null)
        Connection.Dispose();
    }
  }
}



// accessing data

using System.Collections.Generic;
using System.Data.Common;

namespace WidgetScmDataAccess
{
  public class ScmContext
  {
    private DbConnection connection;

    public IEnumerable<PartType> Parts { get; private set; }

    public ScmContext(DbConnection conn)
    {
      connection = conn;
      ReadParts();
    }

    private void ReadParts()
    {
      using (var command = connection.CreateCommand())
      {
        command.CommandText = @"SELECT Id, Name             1
          FROM PartType";
        using (var reader = command.ExecuteReader())        2
        {
          var parts = new List<PartType>();
          Parts = parts;
          while (reader.Read())while (reader.Read())                             3

          {
            parts.Add(new PartType() {                      4
              Id = reader.GetInt32(0),                      5
              Name = reader.GetString(1)                    6
            });
          }
        }
      }
    }
  }

}         


// parameterized queries

public void CreatePartCommand(PartCommand partCommand)
{
  var command = connection.CreateCommand();
  command.CommandText = @"INSERT INTO PartCommand
    (PartTypeId, Count, Command)
    VALUES
    (@partTypeId, @partCount, @command);                             1
    SELECT last_insert_rowid();";                                    2
  AddParameter(command, "@partTypeId", partCommand.PartTypeId);
  AddParameter(command, "@partCount", partCommand.PartCount);
  AddParameter(command, "@command",
    partCommand.Command.ToString());                                 3
  long partCommandId = (long)command.ExecuteScalar();                4
  partCommand.Id = (int)partCommandId;                               5
}

// reading data

public IEnumerable<PartCommand> GetPartCommands()
{
  var command = connection.CreateCommand();
  command.CommandText = @"SELECT
      Id, PartTypeId, Count, Command
    FROM PartCommand
    ORDER BY Id";                                    1
  var reader = command.ExecuteReader();
  var partCommands = new List<PartCommand>();
  while (reader.Read())
  {
    var cmd = new PartCommand() {
      Id = reader.GetInt32(0),
      PartTypeId = reader.GetInt32(1),
      PartCount = reader.GetInt32(2),
      Command = (PartCountOperation)Enum.Parse(      2
        typeof(PartCountOperation),
        reader.GetString(3))
    };
    cmd.Part = Parts.Single(p => p.Id == cmd.PartTypeId);
    partCommands.Add(cmd);
  }

  return partCommands;
}



