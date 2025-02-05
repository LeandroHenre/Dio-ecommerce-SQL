-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema Dio-ecommerce
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema Dio-ecommerce
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `Dio-ecommerce` DEFAULT CHARACTER SET utf8 ;
USE `Dio-ecommerce` ;

-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`Cliente`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`Cliente` (
  `idCliente` INT NOT NULL,
  `Nome` VARCHAR(245) NOT NULL,
  `email` VARCHAR(45) NULL,
  `telefone` VARCHAR(20) NULL,
  `endereco` VARCHAR(245) NULL,
  `tipo` ENUM('Fisica', 'Juridica') NOT NULL,
  PRIMARY KEY (`idCliente`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`Fornecedores`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`Fornecedores` (
  `idFornecedores` INT NOT NULL,
  `Nome` VARCHAR(245) NOT NULL,
  `contato` VARCHAR(245) NULL,
  `telefone` VARCHAR(20) NULL,
  PRIMARY KEY (`idFornecedores`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`categoria`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`categoria` (
  `idcategoria` INT NOT NULL,
  `nome` VARCHAR(245) NOT NULL,
  PRIMARY KEY (`idcategoria`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`produtos`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`produtos` (
  `idprodutos` INT NULL,
  `nome_produtos` VARCHAR(245) NOT NULL,
  `descricao` TEXT(1000) NULL,
  `preco` DECIMAL(10,2) NOT NULL,
  `estoque` INT NULL,
  `categoriaid` INT NULL,
  `fornecedorid` INT NULL,
  PRIMARY KEY (`idprodutos`),
  CONSTRAINT `categoriaid`
    FOREIGN KEY (`idprodutos`)
    REFERENCES `Dio-ecommerce`.`categoria` (`idcategoria`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fornecedorid`
    FOREIGN KEY (`idprodutos`)
    REFERENCES `Dio-ecommerce`.`Fornecedores` (`idFornecedores`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`NotasFiscais`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`NotasFiscais` (
  `idNotasFiscais` INT NOT NULL,
  `idpedido` INT NULL,
  `xml` TEXT(1000) NULL,
  PRIMARY KEY (`idNotasFiscais`),
  CONSTRAINT `idpedido`
    FOREIGN KEY (`idNotasFiscais`)
    REFERENCES `Dio-ecommerce`.`pedidos` (`idpedidos`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`pedidos`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`pedidos` (
  `idpedidos` INT NOT NULL,
  `data` DATE NOT NULL,
  `idcliente` INT NULL,
  `status` VARCHAR(30) NULL,
  `entrega` VARCHAR(45) NULL,
  `pagamento` VARCHAR(45) NULL,
  `notafiscal` INT NULL,
  PRIMARY KEY (`idpedidos`),
  CONSTRAINT `idcliente`
    FOREIGN KEY (`idpedidos`)
    REFERENCES `Dio-ecommerce`.`Cliente` (`idCliente`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `idnotafiscal`
    FOREIGN KEY (`idpedidos`)
    REFERENCES `Dio-ecommerce`.`NotasFiscais` (`idNotasFiscais`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`itenspedido`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`itenspedido` (
  `iditenspedido` INT NOT NULL,
  `idpedido` INT NULL,
  `quantidade` INT NOT NULL,
  `precounitario` DECIMAL(10,2) NOT NULL,
  PRIMARY KEY (`iditenspedido`),
  CONSTRAINT `idpedido`
    FOREIGN KEY (`iditenspedido`)
    REFERENCES `Dio-ecommerce`.`pedidos` (`idpedidos`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `idproduto`
    FOREIGN KEY (`iditenspedido`)
    REFERENCES `Dio-ecommerce`.`produtos` (`idprodutos`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`PessoaFisica`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`PessoaFisica` (
  `idPessoaFisica` INT NOT NULL,
  `cpf` VARCHAR(11) NOT NULL,
  PRIMARY KEY (`idPessoaFisica`),
  CONSTRAINT `idPessoaFisica`
    FOREIGN KEY (`idPessoaFisica`)
    REFERENCES `Dio-ecommerce`.`Cliente` (`idCliente`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Dio-ecommerce`.`PessoaJuridica`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Dio-ecommerce`.`PessoaJuridica` (
  `idPessoaJuridica` INT NOT NULL,
  `cnpj` VARCHAR(14) NOT NULL,
  PRIMARY KEY (`idPessoaJuridica`),
  CONSTRAINT `idPessoaJuridica`
    FOREIGN KEY (`idPessoaJuridica`)
    REFERENCES `Dio-ecommerce`.`Cliente` (`idCliente`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
